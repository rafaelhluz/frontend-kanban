<template>
    <header class ="header">
            <div class="logo">
                <img src="../assets/imgs/freeline.png" alt="HeaderLogo" class="h-8">
            </div>
            <nav class="botoes">
                <router-link to="/login">
                    Recent
                </router-link>
                <router-link to="/login">
                    Starred
                </router-link>
                <router-link to="/login">
                    Templates
                </router-link>
            </nav>

            <input class="campo-busca" type="text" placeholder="Pesquisar..." />

            <img class="notification-link" src="../assets/imgs/notification-icon.png">

            <img class="help-link" src="../assets/imgs/help-icon.png">

    </header>
    
    <header class="header2">
        <h1>Development sprint board</h1>
        <button class="button-share">share</button>
    </header>
</template>

<style scoped>
.header{
    display: flex; /* Define o container como flexível */
    align-items: center; /* Alinha os itens verticalmente ao centro */
    justify-content: space-between; /* Distribui o espaço para empurrar os itens para as extremidades */
}

.botoes {
    display: flex;
    justify-content: center; /*Centraliza os botões horizontalmente*/
    flex-grow: 1; 
}

.logo img {
    height: 20px; /* Ajuste a altura conforme necessário */
}

.botoes router-link {
  margin: 0 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.botoes button:hover {
  background-color: #0056b3;
  color: white;
  transition: 0.5s;
}

.header2 {
    display: flex;
    background-color: #0C1E3EE5;
    color: white;
    padding: 10px 20px;
    font-size: 10px;
}

.campo-busca {
    width: 200px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px; /* Cantos arredondados */
    background-image: url('../assets/imgs/search-Icon.png'); /* Ícone de busca */
    background-size: 16px 16px; /* Tamanho do ícone */
    background-repeat: no-repeat;
    background-position: 5px center; /* Alinha o ícone à esquerda */
    padding-left: 30px; /* Cria espaço para o ícone */
}

.botao-busca:hover {
    background-color: #0056b3;
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2); /* Sombra suave */
}

.button-share {
    margin-left: auto; /* Empurra o botão para a extremidade direita */
    background-color: #007bff; /* Cor de fundo verde */
    color: white; /* Cor do texto branca */
    padding: 8px 40px; /* Espaçamento interno */
    border: none; /* Remove a borda padrão */
    border-radius: 4px; /* Cantos arredondados */
    cursor: pointer; /* Muda o cursor para indicar interatividade */
    transition: background-color 0.3s ease; /* Suaviza a transição de cor */
}

.button-share:hover {
    background-color: #0056b3; /* Cor de fundo ao passar o mouse */
}

.notification-link {
    width: 18px;
    height: 20px;
    margin-left: 20px;
}

.help-link{
    width: 30px;
    height: 30px;
    margin-left: 20px;
}
</style>