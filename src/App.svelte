<script>
    let foodNames = [
        "kebab",
        "pizza",
        "burger",
        "chicken",
        "cheburek",
        "burrito",
        "bagels",
        "subway",
    ];
    let chosenFood = "";
    let foodInput = "";

    function addFood() {
        if (foodNames.indexOf(foodInput) == -1) {
            foodNames = [...foodNames, foodInput];
            foodInput = "";
        }
    }

    function removeFood(foodNameToRemove) {
        foodNames.splice(foodNames.indexOf(foodNameToRemove), 1);
        foodNames = foodNames;
    }

    async function chooseFood() {
        if (foodNames.length) {
            let res = await fetch(`https://www.random.org/integers/?num=1&min=0&max=${foodNames.length - 1}&col=1&base=10&format=plain&rnd=new`);
            let chosenId = await res.text();
            chosenId = chosenId.trim();
            chosenFood = foodNames[chosenId];
        }
    }
</script>

<main>
    <div class="food-list">
        {#each foodNames as foodName (foodName)}
            <div class="food">
                {foodName}
                <button on:click={() => { removeFood(foodName); }}>-</button>
            </div>
        {/each}
    </div>
    <div class="add">
        <input type="text" bind:value={foodInput} />
        <button on:click={addFood}>+</button>
    </div>
    <div class="choose">
        <button on:click={chooseFood}>Choose food</button>
    </div>
    <div class="chosen-food">
        {chosenFood}
    </div>
</main>

<style>
    main {
        width: 100%;
        max-width: 800px;
        margin: 0 auto;
    }

    .food-list {
        border: 1px solid darkgrey;
        padding: 8px;
        margin-bottom: 8px;
    }

    .food {
        border: 1px solid grey;
        padding: 2px;
        margin-bottom: 4px;
    }

    .food button {
        margin: 0;
    }

    .add {
        display: flex;
        margin-bottom: 4px;
    }

    .add input {
        width: 90%;
    }

    .add button {
        width: 10%;
    }

    .choose {
        text-align: center;
        margin-bottom: 4px;
    }

    .choose button {
        background-color: palegreen;
    }

    .chosen-food {
        border: 1px solid orange;
        width: 100%;
        min-height: 120px;
        font-size: 30px;
        color: darkorange;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
