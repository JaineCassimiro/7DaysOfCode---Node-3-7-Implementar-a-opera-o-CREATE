# 7DaysOfCode---Node-3-7-Implementar-a-opera-o-CREATE
7DaysOfCode - Node 3/7: Implementar a operação CREATE
{
  "name": "space-missions-api",
  "version": "1.0.0",
  "description": "API para gerenciamento de missões espaciais - #7DaysOfCode",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "keywords": [
    "api",
    "nodejs",
    "express"
  ],
  "author": "Jaine",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}





// Importa o framework Express
const express = require('express');
// Importa as rotas de missões
const missionRoutes = require('./routes/missionRoutes');

// Cria uma instância do aplicativo Express
const app = express();
// Define a porta em que o servidor irá rodar
const PORT = 3000;

// Middleware para permitir que o Express entenda requisições com corpo em JSON
app.use(express.json());

// Rota principal para verificar se a API está funcionando
app.get('/', (req, res) => {
  res.send('API de Missões Espaciais está no ar!');
});

// Usa as rotas definidas no arquivo missionRoutes para todos os endpoints que começam com /missions
app.use('/missions', missionRoutes);

// Inicia o servidor e o faz escutar na porta definida
app.listen(PORT, () => {
  console.log(`Servidor rodando na porta ${PORT}`);
  console.log(`Acesse http://localhost:${PORT}`);
});






// Simulação de um banco de dados em memória usando um array
let missions = [
    { id: 1, name: 'Apollo 11', year: 1969, country: 'EUA', destination: 'Lua' },
    { id: 2, name: 'Voyager 1', year: 1977, country: 'EUA', destination: 'Espaço Interestelar' },
    { id: 3, name: 'Mars Rover Perseverance', year: 2020, country: 'EUA', destination: 'Marte' }
];
let nextId = 4; // Variável para gerar o próximo ID único

/**
 * Função para criar uma nova missão.
 * Recebe os dados da nova missão e a adiciona ao nosso "banco de dados".
 * @param {object} missionData - Objeto contendo os dados da nova missão (name, year, country, destination).
 * @returns {object} A missão recém-criada com seu novo ID.
 */
const createMission = (missionData) => {
    // Cria um novo objeto de missão com um ID único
    const newMission = {
        id: nextId++,
        ...missionData
    };
    // Adiciona a nova missão ao array
    missions.push(newMission);
    // Retorna a missão que foi criada
    return newMission;
};

// Exporta a função para que ela possa ser usada em outros arquivos
module.exports = {
    createMission
};



// Importa o modelo de missões que interage com os dados
const missionModel = require('../models/missionModel');

/**
 * Função do controlador para criar uma nova missão.
 * É responsável por receber a requisição (req) e enviar a resposta (res).
 * @param {object} req - O objeto de requisição do Express.
 * @param {object} res - O objeto de resposta do Express.
 */
const createMission = (req, res) => {
    // Extrai os dados da nova missão do corpo da requisição (req.body)
    const missionData = req.body;

    // Validação simples para garantir que os campos necessários foram enviados
    if (!missionData.name || !missionData.year || !missionData.country || !missionData.destination) {
        // Se algum campo estiver faltando, retorna um erro 400 (Bad Request)
        return res.status(400).json({ error: 'Todos os campos são obrigatórios: name, year, country, destination.' });
    }

    // Chama a função do modelo para criar a missão no "banco de dados"
    const newMission = missionModel.createMission(missionData);

    // Retorna uma resposta de sucesso com o status 201 (Created)
    // e envia a missão recém-criada como corpo da resposta em formato JSON
    res.status(201).json(newMission);
};

// Exporta a função para que ela possa ser usada nas rotas
module.exports = {
    createMission
}



// Importa o framework Express
const express = require('express');
// Cria uma instância do Router do Express para definir as rotas
const router = express.Router();
// Importa o controlador que contém a lógica para cada rota
const missionController = require('../controllers/missionController');

// Define a rota para a operação CREATE (Criar)
// Quando uma requisição POST for feita para /missions, a função createMission do controlador será executada.
router.post('/', missionController.createMission);

// Exporta o router com as rotas definidas para ser usado no servidor principal
module.exports = router;




Guia Rápido para Testar a Criação de Missão no Postman

Depois de iniciar seu servidor com o comando node server.js, siga estes passos no Postman:

Crie uma Nova Requisição:

Clique no botão + para abrir uma nova aba de requisição.

Configure o Método e a URL:

Mude o método HTTP para POST.

No campo da URL, digite: http://localhost:3000/missions

Monte o Corpo da Requisição (Body):

Abaixo da URL, clique na aba Body.

Selecione a opção raw.

Na caixa de seleção à direita, mude o formato de Text para JSON.

Escreva o JSON da Nova Missão:

Na área de texto, insira os dados da missão que você quer criar. Por exemplo:

{
    "name": "Artemis I",
    "year": 2022,
    "country": "EUA",
    "destination": "Órbita da Lua"
}


Envie a Requisição:

Clique no botão azul Send.

Verifique a Resposta:

Na parte inferior da tela, você deverá ver a resposta do servidor. Se tudo deu certo, o status será 201 Created e o corpo da resposta mostrará a missão que você acabou de criar, agora com um id.

{
    "id": 4,
    "name": "Artemis I",
    "year": 2022,
    "country": "EUA",
    "destination": "Órbita da Lua"
}
