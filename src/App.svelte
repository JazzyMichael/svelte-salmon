<script>
  import { FirebaseApp, User, Doc, Collection } from "sveltefire";

  import firebase from "firebase/app";
  import "firebase/firestore";
  import "firebase/auth";
  import "firebase/performance";
  import "firebase/analytics";

  import firebaseConfig from './firebase.config.js';

  firebase.initializeApp(firebaseConfig);

  import PostForm from './PostForm.svelte';
  import CommentForm from './CommentForm.svelte';

  let viewingPost;
</script>


<main>

  <FirebaseApp {firebase}>

    <h1>💪🔥 Svelte Salmon 🔥💪</h1>

    {#if viewingPost}

      <p>{viewingPost.title}</p>
      <p>{viewingPost.description}</p>
      <p>{new Date(viewingPost.createdAt).toLocaleString()}</p>
      <p>User ID: {viewingPost.userId}</p>
      <p>Post ID: {viewingPost.id}</p>
      
      <button on:click={() => viewingPost = null}>{'<-- View All'}</button>

      <hr />

      <h3>Comments</h3>
      <Collection
        path={'posts/' + viewingPost.id + '/comments'}
        query={ref => ref.orderBy('createdAt').limit(10)}
        let:data={comments}
        let:ref={commentsRef}>

        {#if !comments.length}
            No comments yet...
        {/if}

        {#each comments as comment}
          <p>
            <!-- ID: <em>{comment.ref.id}</em> -->
          </p>
          <p>
            {comment.text}
            <button on:click={() => comment.ref.delete()}>Delete</button>
          </p>
        {/each}

        <br>

        <User let:user let:auth>
          <CommentForm userId={user.uid} on:submit={({ detail }) => commentsRef.add(detail)} />
          <div slot="signed-out">
            <p>💬 you gotta be signed in to comment 💬</p>
            <button on:click={() => auth.signInAnonymously()}>
              Sign In Anonymously
            </button>
          </div>
        </User>
      </Collection>
    {:else}
      <Collection
        path={'posts'}
        query={ref => ref.orderBy('createdAt').limit(10)}
        let:data={posts}
        let:ref={postsRef}>

        {#each posts as post}
          <div on:click={() => viewingPost = post} style="padding: 1em; border: 3px solid black; margin: 1em;">
            <h3>{post.title}</h3>
            <p>{post.description}</p>
            <div style="display: flex; justify-content: space-around; align-items: center;">
              <span>User Id: {post.userId}</span>
              <span>created at {new Date(post.createdAt).toLocaleString()}</span>
            </div>
          </div>
        {/each}

        <br>

        <User let:user let:auth>
          <hr />

          User <em>{user.uid}</em>

          <button on:click={() => auth.signOut()}>Sign Out</button>

          <br><br>

          <PostForm userId={user.uid} on:submit={({ detail }) => postsRef.add(detail)} />

          <div slot="signed-out">
            <button on:click={() => auth.signInAnonymously()}>
              Sign In Anonymously
            </button>
          </div>

          <hr />
        </User>
      </Collection>
    {/if}

  </FirebaseApp>

</main>


<style>
  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto 100px;
  }

  h1,
  em {
    color: #ff3e00;
  }

  hr {
    height: 1px;
    border: none;
    background: rgb(195, 195, 195);
  }
</style>
