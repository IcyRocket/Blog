<script>
	class Post{
		constructor(title, author, isText){
			this.title = title
			this.author = author
			this.isText = isText
		}
	}
	class TextPost extends Post{
		constructor(title, author, content,isText){
			super(title, author,isText)
			this.content = content
		}
	}
	class ImagePost extends Post{
		constructor(title, author,isText){
			super(title, author,isText)
			this.image = ""
		}
	}
	let text = false
	let im = false
	document.addEventListener("DOMContentLoaded", function(){
        let whatPost = document.getElementById("post-select");
		whatPost.addEventListener("change", function(){
			if(this.value == "Text post"){
				text = true
				im = false
			}
			else if(this.value == "Image post"){
				im = true
				text = false
			}
			else if(this.value == ""){
				text = false
				im = false
			}
		})
    });
	let isText = false
	let list = []
	let image = ""
	let title = ""
	let author = ""
	let content = ""
	function loadFile(event) {
		image = event.target.files[0]
	};
	function Submit(event){
		event.preventDefault()
		if(im == true){
			isText = false
			let post = new ImagePost(title, author, isText)
			list.push(post)
			var reader = new FileReader();
			reader.onload = function(){
				list[list.length-1].image = reader.result
			};
			reader.readAsDataURL(image);
			list = list
			console.log(list)
			im = false
		} else if(text == true){
			isText = true
			let post = new TextPost(title, author, content, isText)
			list.push(post)
			text = false
		}
		document.querySelector('#post-select').value = ""
		image = ""
		title = ""
		author = ""
		content = ""
		list = list
	}
	
</script>
<body>
	<main>
		<div class="post-options">
			<h3>What kind of post do you want to make:</h3>
			<div class="select-wrapper">
				<select id="post-select" name="cars">
					<option value=""></option>
					<option value="Text post">Text post</option>
					<option value="Image post">Image post</option>
				</select>
			</div>
			{#if im}
				<form>
					<input type="file"  accept="image/gif, image/jpeg, image/png" name="image" id="file" on:change={loadFile}>
					<h3>Author of your post:</h3>
					<input bind:value={author}>
					<h3>Title of your post:</h3>
					<input bind:value={title}>
					<button on:click={Submit}>Submit</button>
				</form>
			{:else if text}
				<form>
					<label for="title">Title: </label>
					<input bind:value={title} type="text">
					<label for="text">Short statement: </label> 
					<div>
						{#if content.length == 0}
							<div class="gray-custom-field">Write here</div>
						{/if}
						<div class="custom-field" contenteditable="true" bind:textContent={content}></div>

					</div>
					<label for="author">Author:</label>
					<input bind:value={author}>
					<button on:click={Submit} type="submit">Submit</button>
				</form>
			{/if}
		</div>
		<div class="sort-posts">
			{#if list.length > 0}
				{#each list as posts}
					{#if posts.isText === true}
						<div class="inner-post">
							<h1>{posts.title}</h1>
							<p>{posts.content}</p>
							<span class="author-name">This post is made by: {posts.author}</span>
						</div>
					{:else if posts.isText === false}
						<div class="inner-post">
							<h1>{posts.title}</h1>
							<img class="image" src={posts.image} alt="">
							<span class="author-name">This post is made by: {posts.author}</span>
						</div>
					{/if}
				{/each}
			{/if}
		</div>
	</main>
</body>


