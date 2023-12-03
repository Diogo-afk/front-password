<script>
    // onMount é usado para executar uma função quando o componente é montado.
    import { onMount } from 'svelte';
  
    let generatedPassword = "";
    let pass = null;
    let customLength = 7; // Valor padrão para o comprimento personalizado
    let errorMessage = ""; // Mensagem de erro para o comprimento personalizado
  
    // Função assíncrona para gerar a senha com base nos parâmetros de consulta
    const generatePassword = async (queryParams = "") => {
      try {
        // Faz uma requisição à API para gerar a senha
        const response = await fetch(`https://passwordinator.onrender.com${queryParams}`);
        
        // Extrai o texto da resposta
        const data = await response.json();
        
        // Atualiza as variáveis com a senha gerada e os dados da resposta
        pass = data;
        generatedPassword = data.data;
      } catch (error) {
        console.error("Erro ao obter a senha:", error);
        generatedPassword = "Erro ao obter a senha.";
      }
    };
  
    // Função para validar o comprimento personalizado e gerar a senha
    const validateCustomLength = () => {
      if (customLength < 7 || customLength > 18) {
        errorMessage = "O comprimento deve estar entre 7 e 18 caracteres.";
      } else {
        errorMessage = "";
        generatePassword(`?len=${customLength}`);
      }
    };
  
    // Chamada à função generatePassword
    onMount(() => {
      generatePassword();
    });
  
    // Funções para personalizar a geração da senha
    const addNumber = () => {
      generatePassword("?num=true");
    };
  
    const addSpecialChar = () => {
      generatePassword("?char=true");
    };
  
    const addCaps = () => {
      generatePassword("?caps=true");
    };
  
    const generateCustomChars = () => {
      validateCustomLength();
    };
  </script>
  
  <main>
    <h1 class="tituloGerar">Gerador de Senha</h1>
  
    <!-- Exibição da senha gerada -->
    {#if generatedPassword}
      <p class="senhaGerada">Senha Gerada: {pass.data}</p>
    {/if}
  
    <!-- Botões para personalizar a geração da senha -->
    <div class="button-container">
      <button on:click={addNumber} class="button button1">Add Numero</button>
      <button on:click={addSpecialChar} class="button button2">Add Caracter Especial</button>
      <button on:click={addCaps} class="button button3">Add Letra Maiuscula</button>
    </div>
  
    <!-- Container para comprimento personalizado -->
    <div class="custom-length-container">
      <!-- Input para inserção do comprimento personalizado -->
      <input type="number" bind:value={customLength} />
      
      <!-- Exibição da mensagem de erro, se houver -->
      {#if errorMessage}
        <p class="error-message">{errorMessage}</p>
      {/if}
  
      <!-- Botão para gerar senha personalizada -->
      <button on:click={generateCustomChars} class="button button4">Senha Personalizada</button>
    </div>
  </main>
  
  <style>
    main {
      text-align: center;
      margin-top: 2rem;
    }
  
    p {
      margin-top: 1rem;
    }
  
    div {
      margin-top: 1rem;
    }
  
    button {
      border: none;
      border-radius: 12px;
      color: white;
      padding: 16px 24px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 4px 2px;
      transition-duration: 0.4s;
      cursor: pointer;
      width: 30%;
    }
  
    .button-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 1rem;
    }
  
    .custom-length-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 1rem;
    }
  
    .error-message {
      color: red;
    }
  
    .button1 {
      background-color: white; 
      color: black; 
      border: 2px solid #04AA6D;
    }
  
    .button1:hover {
      background-color: #04AA6D;
      color: white;
    }
  
    .button2 {
      background-color: white; 
      color: black; 
      border: 2px solid #008CBA;
    }
  
    .button2:hover {
      background-color: #008CBA;
      color: white;
    }
  
    .button3 {
      background-color: white; 
      color: black; 
      border: 2px solid #f44336;
    }
  
    .button3:hover {
      background-color: #f44336;
      color: white;
    }
  
    .button4 {
      background-color: white; 
      color: black; 
      border: 2px solid #555555;
    }
  
    .button4:hover {
      background-color: #555555;
      color: white;
    }
  
    h1.tituloGerar {
      font-size: 3.5em;
    }
  
    p.senhaGerada {
      font-size: 2.0em;
    }
  </style>
  