<script>
    let foods = window.localStorage.getItem('foods') ? JSON.parse(window.localStorage.getItem('foods')) : [];
    $: sum = foods.reduce((acc, curFood) => acc + curFood.points, 0);

    let chosenFood = "";
    let foodInput = "";

    function updateStorage() {
        localStorage.setItem('foods', JSON.stringify(foods));
    }

    function resetPoints() {
        foods.forEach((food) => food.points = foods.length);
        foods = foods;
        updateStorage();
    }

    function addFood() {
        if (foods.every((food) => foodInput !== food.name)) {
            foods.push({name: foodInput, points: foods.length});
            foods.forEach((food) => food.points++);
            foods = foods;
            foodInput = "";
            updateStorage();
        }
    }

    function removeFood(foodNameToRemove) {
        foods.splice(foods.findIndex((food) => food.name === foodNameToRemove), 1);
        foods.forEach((food) => {
            if (food.points > 0) {
                food.points--;
            }
        });
        foods = foods;
        updateStorage();
    }

    async function chooseFood() {
        if (sum > 1) {
            let res = await fetch(`https://www.random.org/integers/?num=1&min=1&max=${sum}&col=1&base=10&format=plain&rnd=new`);
            let chosenNumber = await res.text();
            chosenNumber = +chosenNumber.trim();
            let collectedPoints = 0;
            for (const food of foods) {
                if (chosenNumber > collectedPoints && chosenNumber <= collectedPoints + food.points) {
                    chosenFood = food.name;
                    food.points--;
                    break;
                }
                collectedPoints += food.points;
            }
            foods = foods;
            updateStorage();
        }
    }
</script>

<main>
    <div class="food-list">
        {#each foods as food (food.name)}
            <div class="food">
                <strong>{food.name}</strong>
                ({food.points} / {sum} = {Math.round((food.points / sum) * 100)}%)
                <button on:click={() => { removeFood(food.names); }}>-</button>
            </div>
        {/each}
    </div>
    <div class="reset center">
        <button on:click={resetPoints}>Reset points</button>
    </div>
    <div class="add">
        <input type="text" bind:value={foodInput} />
        <button on:click={addFood}>+</button>
    </div>
    <div class="choose center">
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

    .center {
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
