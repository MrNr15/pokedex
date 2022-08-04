<script context="module">
    
    let pokemon
    let current_sprite
    let shiny = false
    let abilities = []
    let types = []
    let stats = []
    
    export async function load({url2,params}){
        const id = params.id
        const url = "https://pokeapi.co/api/v2/pokemon/" + id.toString()

        const res = await fetch(url)
        const data = await res.json()

        pokemon = data
        current_sprite = pokemon.sprites.front_default

        abilities = pokemon.abilities.map((data, index) => {
            return capitalizedWord(data.ability.name)
        })

        types = pokemon.types.map((data, index) => {
            return capitalizedWord(data.type.name)
        })

        stats = pokemon.stats.map((data, index) => {
            return data.base_stat
        })

    }

    function capitalizedWord(word){
        return word.charAt(0).toUpperCase() + word.slice(1)
    }

    function toggle_shiny(){
        shiny = !shiny
        if (shiny){
            current_sprite = pokemon.sprites.front_shiny
        }else{
            current_sprite = pokemon.sprites.front_default
        }
        document.getElementById("image").src = current_sprite
    }
</script>

<script>
    
    import { onMount } from 'svelte';
    import Footer from "../footer.svelte"
    import Header from "../header.svelte"
    
    onMount(() => {

        
        let type_texts = document.querySelectorAll(".type")

        for(let i = 0; i < type_texts.length; i++){

            let color = "grey"
            let black_text = false
        if (types[i] == "Grass"){
            color = "green"
        } if (types[i] == "Poison"){
            color = "purple"
        } if (types[i] == "Fire"){
            color = "red"
        } if (types[i] == "Water"){
            color = "blue"
        } if (types[i] == "Electric"){
            color = "yellow"
            black_text = true
        } if (types[i] == "Ice"){
            color = "aqua"
        } if (types[i] == "Fighting"){
            color = "brown"
        } if (types[i] == "Ground"){
            color = "darkkhaki"
            black_text = true
        } if (types[i] == "Flying"){
            color = "cornflowerblue"
            black_text = true
        } if (types[i] == "Psychic"){
            color = "palevioletred"
        } if (types[i] == "Bug"){
            color = "chartreuse"
            black_text = true
        } if (types[i] == "Rock"){
            color = "burlywood"
            black_text = true
        } if (types[i] == "Ghost"){
            color = "rebeccapurple"
        } if (types[i] == "Dragon"){
            color = "indigo"
        } if (types[i] == "Fairy"){
            color = "pink"
            black_text = true
        }

        type_texts[i].style.background = color
        type_texts[i].style.color = "white"
        if (black_text){
            type_texts[i].style.color = "black"
        }

        }

    })

</script>

<Header></Header>

<div class="main">
    <div class="background">
        <h1>{capitalizedWord(pokemon.name)}</h1>
        <div class="steckbrief">
            <div class="image-side">
                <div class="image">
                    <img src={current_sprite} alt="" id="image">
                    <button on:click={toggle_shiny}>Shiny</button>
                </div>
            </div>
            <div class="information">
                <p>Name: {capitalizedWord(pokemon.name)}</p>
                <p>Id: {pokemon.id}</p>
                <p>Height: {pokemon.height}</p>
                <p>Weight: {pokemon.weight}</p>
                <p class="title">Types</p>
                <div class="align-horizontal">
                    {#each types as type}
                    <div class="type" id="type">
                        {type + " "}
                    </div>
                    {/each}
                </div>
                <p class="title">Abilitys</p>
                <div class="align-horizontal">
                    {#each abilities as ability}
                    <div class="ability">
                        {ability + " "}
                    </div>
                    {/each}
                </div>

                <div class="stats">
                    <p class="title">Stats</p>
                    Hp: {stats[0]} <br>
                    Attack: {stats[1]} <br>
                    Defence: {stats[2]} <br>
                    Special-Attack: {stats[3]} <br>
                    Special-defence: {stats[4]} <br>
                    Speed: {stats[5]}
                </div>

                <br>
            </div>
        </div>
    </div>
</div>

<Footer></Footer>

<style>

    :global(html){
        overflow-x: hidden;
        font-family: sans-serif;
    }

    
    .background{
        background-color: var(--color-1);
        padding: 1em 3em 1em 3em;
        color: white;
        margin: 25vh 0;
        
        font-size: 1.5em;
    }
    
    .steckbrief{
        display: flex;
        flex-direction: row;
    }

    h1{
        margin: 0.2em 0 0.5em 0;
        text-align: center;
    }
    
    img{
        background-color: var(--color-2);
        image-rendering: pixelated;
        width: 300px;
        border-radius: 25px;
        margin-bottom: 10px;
    }
    
    .image{
        display: grid;
        justify-items: center;
        margin-right: 100px;
    }
    
    button{
        max-width: 300px;
        height: 2em;
        width: 50%;
        font-size: 1em;
        border-radius: 50px;
        border: solid;
        border-width: 5px;
        border-color: var(--color-5);
    }

    .stats{
        background-color: var(--color-2);
        padding: 0 1em 0 1em;
        color: black;
    }
    
    .align-horizontal{
        margin:  0;
        display: grid;
        display: flex;
        flex-wrap: wrap;
        grid-template-columns: repeat(auto-fit, minmax(50px, 120px));
        justify-content: center;
        margin: 0;
        margin-bottom: 1em;
    }
    
    .align-horizontal > div{
        padding: 0 0.5em 0 0.5em;
        border-radius: 3em;
        background-color: var(--color-5);
        color: black;
        margin: 0.2em;
    }

    .title{
        margin: 0;
        text-align: center;
        font-size: 1.2em;
    }
    
    @media only screen and (min-width: 900px){
        .main{
            width: 100%;
            display: flex;
            align-items: center;
            flex-direction: column;
            background-color: var(--color-6);
        }

        .border{
            height: 15vh;
        }
    }
    
    @media only screen and (max-width: 900px){
        
        .image{
            display: grid;
            justify-items: center;
            margin: 0px;
            width: 100%;
        }
        
        img{
            width: 100%;
            max-width: 300px;
        }
        
        .steckbrief{
            flex-direction: column;
        }

        .background{
            padding: 1em 0.3em;
            margin: 0;
        }
        
    }

</style>