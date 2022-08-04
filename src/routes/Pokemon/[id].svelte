<script context="module">
    
    let pokemon
    let current_sprite
    let shiny = false
    let abilities = []
    let types = []
    
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
    
    onMount(() => {

        
        let type_texts = document.querySelectorAll(".type")
        console.log(types)

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
            color = "pink"
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

        console.log(document.querySelectorAll(".type"))

        }

    })

</script>

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
                <p>weight: {pokemon.weight}</p>
                <br>
                <p>Abilitys: </p>
                <div class="align-horizontal">
                    {#each abilities as ability}
                    <div class="ability">
                        {ability + " "}
                    </div>
                    {/each}
                </div>
                <br><br>
                <p>Types: </p>
                <div class="align-horizontal">
                    {#each types as type}
                    <div class="type" id="type">
                        {type + " "}
                    </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>

<style>

    :root{
        --normal: #ffffff;
        --flying: #ffffff;
        --grass: #26ff00;
        --poison: #ffffff;
        --fire: #ffffff;
        --water: #ffffff;
        --bug: #ffffff;
        --fairy: #ffffff;
        --psychic: #ffffff;
        --electric: #ffffff;
        --ice: #ffffff;
        --fighting: #ffffff;
        --ground: #ffffff;
        --rock: #ffffff;
        --ghost: #ffffff;
        --dragon: #ffffff;
        --type: #000000;

        --color-1: #283044;
        --color-2: #78a1bb;
        --color-3: #ebf5ee;
        --color-4: #bfa89e;
        --color-5: #8b786d;
    }

    
    .background{
        background-color: var(--color-1);
        padding: 1em 3em 1em 3em;
        color: white;
        
        font-size: 1.5em;
    }
    
    .steckbrief{
        display: flex;
        flex-direction: row;
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
    
    .align-horizontal{
        margin:  0 auto;
        display: grid;
        display: flex;
        flex-wrap: wrap;
        grid-template-columns: repeat(auto-fit, minmax(50px, 120px));
        justify-content: center;
        margin: 0;
    }
    
    .align-horizontal > div{
        padding: 0 0.5em 0 0.5em;
        border-radius: 3em;
        background-color: var(--color-2);
        color: black;
        margin: 0.2em;
    }
    
    @media only screen and (min-width: 900px){
        .main{
            width: 100%;
            height: 90vh;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
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
            padding: 1em 1em 1em 1em;
        }
        
    }

</style>