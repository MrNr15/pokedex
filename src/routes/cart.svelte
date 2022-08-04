<script>
    export let item = {
        name: "test item"
    }

</script>

<div class="cart">
    <a href={"/Pokemon/" + item.id}>
        <img class="image" src={item.image} alt="">
        {item.cap_name}
    </a>
</div>

<style>

    a{
        text-decoration: none;
        color: black;
    }

    .cart{
        width: 300px;
        background-color: var(--color-2);
        color: white;
        font-size: 2em;
        text-align: center;
        color: black;
        text-decoration: none;
        border-radius: 35px;
    }

    .cart:hover{
        box-shadow: 0 0 10px black;
    }

    .image{
        width: 100%;
        image-rendering: pixelated;
    }

</style>