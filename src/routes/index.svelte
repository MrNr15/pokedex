<script>

    import Footer from "./footer.svelte"
    import Header from "./header.svelte"

    function capitalizedWord(word){
        return word.charAt(0).toUpperCase() + word.slice(1)
    }

    let pokemon = []
    let search_pokemon = []
    let shown_pokemon = []

    let searching = false
    let results = 150

    const fetchPokemon = async () => {
        const url = "https://pokeapi.co/api/v2/pokemon?limit=150"
        const res = await fetch(url)
        const data = await res.json()
        const loadedPokemon = data.results.map((data, index) => {
            return {
                name: data.name,
                cap_name: capitalizedWord(data.name),
                id: index + 1,
                image: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/" + (index+1).toString() + ".png"
            }
        })

        pokemon = loadedPokemon
        search_pokemon = pokemon
        shown_pokemon = pokemon.slice(0,20)

        search()
    }

    fetchPokemon()

    function search_array(array, search){
        let result = []
        for (let i = 0; i < array.length; i++){
            if (array[i].name.indexOf(search) != -1){
                result.push(array[i])
            }
        }
        return result
    }

    function search(){
        searching = true
        page = 1
        search_pokemon = search_array(pokemon, search_name.toLowerCase())
        shown_pokemon = search_pokemon.slice(page*20-20, page*20)
        results = search_pokemon.length
    }

    import Gamecart from './cart.svelte';

    let page = 1

    let search_name = ""

    function next(){

        if (searching && page*20 < search_pokemon.length){
            page += 1
            shown_pokemon = search_pokemon.slice(page*20-20, page*20)
        }
        if (!searching && page*20 < pokemon.length){
            page += 1
            shown_pokemon = pokemon.slice(page*20-20, page*20)
        }
    }

    function prev(){
        if (searching && page > 1){
            page -= 1
            shown_pokemon = search_pokemon.slice(page*20-20, page*20)
        }
        if (!searching && page > 1){
            page -= 1
            shown_pokemon = pokemon.slice(page*20-20, page*20)
        }
    }

    import { onMount } from 'svelte';

    onMount(() => {  

        document.getElementById("search").addEventListener("keypress", function(event){
            if (event.keyCode === 13){
                search()
            }
        })
    })
    
</script>

<Header></Header>

<div class="background">
    
    <div class="searchbar">
        <input bind:value={search_name} type="text" class="search" id="search" placeholder="Search Pokemon" on:input={search}>
    </div>
    
    <div class="grid">
        {#each shown_pokemon as item}
        <Gamecart item={item}/>
        {/each}
    </div>
    
    <div class="pages">
        Showing {page*20-19}-{page*20-20 + shown_pokemon.length} of {results} results
        {#if page > 1}
        <button on:click={prev}>prev</button>
        {/if}
        {#if page*20 < search_pokemon.length}
        <button on:click={next}>next</button>
        {/if}
    </div>
</div>

    <Footer></Footer>

<style>
        
    .grid{
        display: grid;
        grid-template-columns: repeat(auto-fit, min(300px));
        grid-column-gap: 1rem;
        grid-row-gap: 2rem;
        justify-content: center;
        justify-items: center;
    }
    .background{
        background-color: var(--color-1);
        margin: 0;
    }

    .searchbar{
        padding: 1em 4em;
    }


    .search{
        border-radius: 20px;
        border: solid;
        border-width: 4px;
        border-color: lightgray;
        font-size: 1.3em;
    }

    .pages{
        font-size: 1.5em;
        padding: 0.5em 2em 0.5em 2em;
        color: white;
    }
    .pages > button{
        font-size: 1em;
        border-radius: 10px;
        border: solid;
        border-width: 3px;
        border-color: darkgray;
    }

    @media only screen and (max-width: 900px){

        .pages{
            font-size: 1.5em;
            padding: 0.25em 0.5em;
        }

        .searchbar{
        display: flex;
        justify-content: center;
        padding: 1em 0;
        }

    .search{
        font-size: 1.4em;
        width: 90%;
    }

    }

</style>