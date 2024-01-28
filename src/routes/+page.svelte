<script lang="ts">
    import axios from "axios";
    import {onMount, onDestroy, afterUpdate, beforeUpdate} from "svelte";
    import {fade, fly} from "svelte/transition"
 
    let term = "";
    let photos: {
        id: string;
        alt_description: string;
        urls: {regular: string};
    }[] = [];


    const fetchData = async () => {
        const response = await axios.get(

        `https://api.unsplash.com/search/photos?page=1&query=${term || "office"}&client_id=O_xjy_tcNlcpi6yhWdpOt-k6j5Y5IjsTjQqKBAZyOp8`

        );

        photos = response.data.results;
    };

    // https://api.unsplash.com/search/photos?page=1&query=office&client_id=cNdZT4jgLO5wyg2mUWPGvdjYPpLGbYhdKz3Or7sVokg

    onMount(() => {
       fetchData();
    })

    onDestroy(() => {
        console.log("goodbye there")

    })

    beforeUpdate(() =>{
        console.log("I ran before the app updated")
    })

    beforeUpdate(() => {
        console.log("I ran after the app updated");
    })

    const handleSearch = async () => {
        if(!term) return;
        await fetchData();
        // term = 
    }

</script>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Gallery</title>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Image Gallery</h1>
        </div>

        <div class="input-container">
            <input type="text" class="input" placeholder="Type a category" bind:value={term}/>
            <button class="btn" on:click="{handleSearch}">Search</button>
        </div>
        
        <div class="photos">
          {#each photos as photo, i (photo.id)}
            <img src={photo.urls.regular} 
            alt={photo.alt_description} 
            class="image"
             in:fly={{y: 200 , duration: 2000, delay: i * 200}}
             out:fade
             />

          {/each}
        </div> 
    </div>
</body>
</html>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Rubik:ital@0;1&display=swap');

    *{
        font-family: 'Rubik Doodle Shadow', system-ui;
    }

    body{
        /* background: linear-gradient(180deg, #345392, #750a90); */
        height: 100%;
        width: 100%;
        background: linear-gradient(120deg, #373737, #8b8a8a ) ;
    }

    .container{
        display: grid;
        place-items: center;
        align-items: center;
        margin: 0 auto;
    }

    .header{
        text-align: center;
        color: #fff;

    }

    .input{
        outline: none;
        width: 250px;
        height: 20px;
        padding: 10px;
        border-radius: 5px;
        border: 2px solid #fff;
        font-size: 18px;
        background: transparent;
        color: #fff;
    }

    .input::placeholder{
        color: #fff;
    }

    .btn{
       padding: 10px 10px;
       outline: none;
       border: none;
       width: 100px;
       background: #ff5945;
       border-radius: 5px;
       font-size: 18px;
       font-weight: 500;
       cursor: pointer;
       color: #fff;
    }

    .btn:hover{
        background: #f88174;
        transition: all 0.2s;
        color: #212121;
    }

    .image{
        width: 300px;
        height: 300px;
        margin: 10px;
        border-radius: 6px;
    }

    .photos{
        display: grid;
        grid-template-columns: repeat(3, 3fr);
    }

    .input-container{
        margin-bottom: 40px;
    }

    @media (max-width: 480px){ 
        /* body { 
            background-color: red; 
        }  */

    .photos{
        display: grid;
        grid-template-columns: repeat(2, 2fr);
    }

    .image{
        width: 150px;
        height: 150px;
        margin: 5px;
        border-radius: 10px;
    }

    .input{
        width: 180px;
        height: 20px
    }

    .header h1{
      font-size: 18px;

    }

}

/* Media Query for low resolution Tablets, Ipads */ 
 @media (min-width: 481px) and (max-width: 767px) { 
     /* body { 
        background-color: yellow; 
    } */

    .photos{
        display: grid;
        grid-template-columns: repeat(2, 2fr);
    }

    .image{
        width: 170px;
        height: 170px;
        margin: 5px;
        border-radius: 10px;
    }

    .input{
        width: 190px;
        height: 20px
    }

    .header h1{
      font-size: 18px;

    }

   
} 

/* Media Query for Tablets Ipads portrait mode */ 
@media (min-width: 768px) and (max-width: 1024px){ 
    /* body { 
        background-color: blue; 
    } */


    .photos{
        display: grid;
        grid-template-columns: repeat(2, 2fr);
    }

    .image{
        width: 250px;
        height: 250px;
        margin: 5px;
        border-radius: 10px;
    }

    .input{
        width: 250px;
        height: 20px
    }

    .header h1{
      font-size: 18px;

    }


} 

/* Media Query for Laptops and Desktops */ 
 @media (min-width: 1025px) and (max-width: 1280px){ 
     /* body { 
        background-color: green; 
    } */

    
}

/* Media Query for Large screens */ 
@media (min-width: 1281px) { 
   
} 
            
</style>