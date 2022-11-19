<script>
    import ToggleInput from "./ToggleInput.svelte";

    const taxesData = [
        {
            level: 1,
            year: 77400,
            month: 6450,
            tax: 0.10,
            postSalaryMax: 5805.86,
            postSalaryConst: 0
        },
        {
            level: 2,
            year: 33480,
            month: 2790,
            tax: 0.14,
            postSalaryMax: 8205.2,
            postSalaryConst: 258
        },
        {
            level: 3,
            year: 67200,
            month: 5600,
            tax: 0.20,
            postSalaryMax: 12685.09,
            postSalaryConst: 812.4
        },
        {
            level: 4,
            year: 69360,
            month: 5780,
            tax: 0.31,
            postSalaryMax: 16673.25,
            postSalaryConst: 2444.8
        },
        {
            level: 5,
            year: 267480,
            month: 22290,
            tax: 0.35,
            postSalaryMax: 31161.1,
            postSalaryConst: 3269.6
        },
        {
            level: 6,
            tax: 0.47,
            postSalaryMax: 36580.03,
            postSalaryConst: 8418.27
        },
    ];
    const fixedNumber = value => Number(Number(value).toFixed(2))
    const calculateTax = (value, depth = 1) => {
        if (depth > 5 || value <= taxesData[depth - 1][currencyPerMonth ? 'month' : 'year']) {
            return fixedNumber(value * taxesData[depth - 1].tax)
        }
        let valueTemp = (value - taxesData[depth - 1][currencyPerMonth ? 'month' : 'year']);
        let taxTemp = taxesData[depth - 1][currencyPerMonth ? 'month' : 'year'] * taxesData[depth - 1].tax
        const argument = depth === 1 && value > (currencyPerMonth ? 55270 : 663240) ? 1.03 : 1;
        const result = (taxTemp + calculateTax(valueTemp, depth + 1)) * argument
        return fixedNumber(result)
    }
    const calculatePreTaxSalary = value => {
        const argument = currencyPerMonth ? 1 : 12;
        for (let i = 0; i < taxesData.length; i++) {
            if (value < taxesData[i].postSalaryMax * argument) {
                return fixedNumber((value - taxesData[i].postSalaryConst * argument)/(1 - taxesData[i].tax))
            }
        }
        return fixedNumber((value - 8165.72 * argument) / 0.514)
    }
    const handleInputChange = () => {
        inputIsPreTax = !inputIsPreTax;
        salaryPreTax = 0;
        salaryPostTax = 0;
        tax = 0;
    }

    $: inputIsPreTax = true;
    $: currencyPerMonth = true;
    $: salaryPreTax = 0;
    $: salaryPostTax = 0;
    $: tax = 0;

    $: if (inputIsPreTax && (currencyPerMonth || !currencyPerMonth)) {
        tax = calculateTax(salaryPreTax);
        salaryPostTax = fixedNumber(salaryPreTax - tax);
    } else {
        salaryPreTax = calculatePreTaxSalary(salaryPostTax);
        tax = fixedNumber(salaryPreTax - salaryPostTax);
    }
</script>

<div class="TaxCard">
    <p class="TaxCard__description">
        Find out your salary before or after paying taxes!<br>
        You can toggle the time period between «per month» and «per year» and choose an active salary input method in the settings below.
    </p>
    <div class="TaxCard__settings">
        <ToggleInput
            option1="pre-tax"
            option2="post-tax"
            value="{inputIsPreTax}"
            callback="{handleInputChange}"
        />
        <ToggleInput
            option1="per month"
            option2="per year"
            value="{currencyPerMonth}"
            callback="{() => currencyPerMonth = !currencyPerMonth}"
        />
    </div>
    <div class="d-flex align-center">
        <div
            class="TaxCard__transformable"
            class:--down={!inputIsPreTax}
        >
            Pre-tax salary:
        </div>
        {#if inputIsPreTax}
            <input
                type="number"
                class="input TaxCard__tax--margin"
                bind:value={salaryPreTax}
            >
        {:else}
            <input
                type="number"
                class="input TaxCard__tax--margin"
                bind:value={salaryPostTax}
            >
        {/if}

    </div>
    <div class="d-flex">
        <div
            class="TaxCard__transformable"
            class:--up={!inputIsPreTax}
        >
            Post-tax salary:
        </div>
        <div class="TaxCard__tax--margin">{inputIsPreTax ? salaryPostTax : salaryPreTax}</div>
    </div>
    <div>The tax is {tax}</div>
</div>

<style>
    .TaxCard {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    .TaxCard__description {
        margin-top: 0;
        margin-bottom: 0;
    }
    .TaxCard__settings {
        display: flex;
        gap: 10px;
    }
    .TaxCard__transformable {
        transform: translateY(0);
        transition: transform 0.3s ease-in-out;
    }
    .TaxCard__transformable.--down {
        transform: translateY(43px);
    }
    .TaxCard__transformable.--up {
        transform: translateY(-43px);
    }
    .TaxCard__tax--margin {
        margin-left: 20px;
    }
    @media (max-width: 512px) {
        .TaxCard__settings {
            flex-wrap: wrap;
        }
    }
</style>
