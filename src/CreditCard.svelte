<script>
	let cardNumber = '',
			name = '',
			logo = '',
			month = '',
      year = '',
      formCheck = false,
      error = false,
      errorMonth = false,
      errorYear = false,
      errorCvv = false,
      errorMessage = ''

	$: firstNumber = parseInt(cardNumber[0])
	$: nameUpper = name.trim().toUpperCase()
	
	const changeBackground = () => {
    let numberLength = cardNumber.length
    const card = document.getElementById('card')
    
		switch (firstNumber) {
			case 3:
				card.style.setProperty('--bg-color', '#6DAC97')
				logo = 'american'
				break;
			case 4:
				card.style.setProperty('--bg-color', '#019FC7')
				logo = 'visa'
				break;
			case 5:
				card.style.setProperty('--bg-color', '#826423')
				logo = 'master-card'
				break;
			default:
				card.style.setProperty('--bg-color', '#9F9F9F')
				logo = ''
				break;
    }
    
    if (numberLength == 4 || numberLength == 9 || numberLength == 14) cardNumber += ' '

    resetErrors()
  }

  const formOk = () => {
    formCheck = !formCheck
  }

  const resetErrors = () => {
    error = false
    errorMonth = false
    errorYear = false
    errorCvv = false
  }

  const validateNumbers = () => {
    let cardNumber = document.getElementById('cardNumber').value,
        cardName = document.getElementById('cardName').value,
        cardMonth = document.getElementById('cardMonth').value,
        cardYear = document.getElementById('cardYear').value,
        cardCvv = document.getElementById('cardCvv').value
    
    console.log(cardNumber)
    if (cardNumber = '' || cardNumber.length < 19 ) {
      error = !error
      errorMessage = 'El número no esta completo'
      return false
    } else if (cardMonth = '' || cardMonth.length !== 2 || isNaN(cardMonth)) {
      errorMonth = !errorMonth
      errorMessage = 'Hay un error con la fecha'
      return false
    } else if (cardYear = '' || cardYear.length !== 4 || isNaN(cardYear)){
      errorYear = !errorYear
      errorMessage = 'Complete la fecha adecuadamente'
      return false
    }else if(cardCvv = '' || cardCvv.length !== 3 || isNaN(cardCvv)) {
      errorCvv = !errorCvv
      errorMessage = 'El campo CVV debe tener 3 digitos'
      return false
    }

    formCheck = !formCheck
  }
</script>


<style>
	* { box-sizing: border-box; }

  small {
    font-size: .8rem;
  }

	img {
		display: block;
		max-width: 100%;
	}
	
  figure {
    margin-top: 0;
    margin-bottom: 0;
  }
  
	.component {
    --section-height: 250px;
    --primary-color: hsl(100, 50%, 50%);
    --hover: hsl(100, 50%, 40%);
    --error-color: hsl(0, 60%, 50%);
		max-width: 80%;
		margin-right: auto;
		margin-left: auto;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
    font-family: 'Roboto', sans-serif;
    font-size: 1.2rem;
    font-weight: 500;
	}	
	
	.card {
		--bg-color: #9F9F9F;
		font-family: 'Consolas', sans-serif;
		flex-basis: 400px;
		height: var(--section-height);
		margin-right: 2rem;
		margin-bottom: 2rem;
		padding: 2rem 1rem;
		background-color: var(--bg-color);
		border-radius: 1.5rem;
	}

	.card__header,
  .form__group {
		display: flex;
    justify-content: space-between;
		align-items: center; 
	}

	.card__header { height: 4rem; }

	.card__img { width: 100px; }

	.card__img:first-child { border-radius: 10px; }

	.form { flex-basis: 300px; }

	.form__month { width: 50px; }

	.form__year,
	.form__cvv--input { width: 100px; }

	.form__button { 
    margin-bottom: 0;
    color: #FFF;
    background-color: var(--primary-color);
    cursor: pointer;
    border-radius: 5px;
  }

  .form__button:hover { background-color: var(--hover); }

  .ok-message {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: var(--section-height);
    width: 200px;
    color: var(--primary-color);
  }

  .block { 
    display: block;
    margin: 1rem 0;
  }

	.full { width: 100%; }

  .font-big {
    font-size: 1.5rem;
  }

	.center { text-align: center; }

	.ov-hidden { overflow: hidden; }

  .error {
    border-color: var(--error-color);
  }

  .error-message {
    display: block;
    margin-top: -.5rem;
    margin-bottom: 1.5rem;
    color: var(--error-color)
  }
</style>

<section class="component">
	<h1 class="full center">Nuevo método de pago</h1>
	<div class="card" id="card">
		<div class="card__header ov-hidden">
			<div class="card__img ov-hidden">
				<img src="./img/chip.PNG" alt="Card chip">
			</div>
			{#if logo !== ''}
				<div class="card__img">
					<img src="./img/{logo}-logo.png" alt="Card logo">
				</div>
			{/if}
		</div>
		<span class="block font-big">{cardNumber}</span>
		<span class="block">{nameUpper}</span>
			<span>{month}</span>
		{#if year !== ''}
			<span>/</span>
			<span>{year}</span>
		{/if}
	</div>
  {#if !formCheck}
    <form id="form" class="form" action="#" on:submit|preventDefault={validateNumbers}>
      <label for="cardNumber">Número de tarjeta</label>
      <input bind:value="{cardNumber}" type="text" id="cardNumber" class="form__number full" class:error on:keyup="{changeBackground}" maxlength="19" required>
      {#if error}
        <span class="error-message"><small>{errorMessage}</small></span>
      {/if}
      <label for="name">Nombre del tarjetahabiente</label>
      <input type="text" id="cardName" class="form__name full" bind:value="{name}" required>
      <div class="form__group">
        <div>
          <label for="date" class="form__date--label">Fecha de vencimiento</label>
          <input bind:value="{month}" type="text" placeholder="MM" id="cardMonth" class="form__month" class:error={errorMonth} maxlength="2" on:keyup="{resetErrors}" required>
          <input bind:value="{year}" type="text" id="cardYear" placeholder="AAAA" class="form__year" class:error={errorYear} maxlength="4" on:keyup="{resetErrors}" required>
        </div>
        <div>
          <label for="cvv" class="form__cvv--label">Código CVV</label>
          <input type="text" id="cardCvv" class="form__cvv--input" class:error={errorCvv} maxlength="3" on:keyup="{resetErrors}" required>
        </div>
      </div>
        {#if errorYear || errorMonth || errorCvv}
            <span class="error-message block"><small>{errorMessage}</small></span>
          {/if}
      <input type="submit" value="Agregar método de pago" class="form__button full">
    </form>
  {:else}
    <figure class="ok-message center" id="okMessage">
      <img src="./img/check.png" alt="Icon form ok">
      <figcaption>
        <p>El método de pago se agregó exitosamente</p>
      </figcaption>
    </figure>
  {/if}
</section>

