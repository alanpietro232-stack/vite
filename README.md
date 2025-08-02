<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Sistema Pousada Bicharada</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    h2 { color: #333; }
    form, table { margin-bottom: 30px; }
    table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 8px; }
    button { padding: 6px 12px; margin-top: 5px; }
  </style>
</head>
<body>
  <h1>Sistema Pousada Bicharada</h1>

  <!-- Cadastro Pet e Tutor -->
  <h2>Cadastro de Pet e Tutor</h2>
  <form id="cadastroForm">
    <input type="text" placeholder="Nome do Pet" id="petNome" required>
    <input type="text" placeholder="Nome do Tutor" id="tutorNome" required><br><br>
    <label>Porte:</label>
    <select id="porte">
      <option value="P">P</option>
      <option value="M">M</option>
      <option value="G">G</option>
    </select>
    <input type="text" placeholder="Espécie" id="especie">
    <input type="text" placeholder="Comida (ração/natural)" id="comida">
    <input type="text" placeholder="Horário da comida" id="horario">
    <input type="text" placeholder="Medicação" id="medicacao">
    <input type="text" placeholder="Vacinas" id="vacinas">
    <input type="text" placeholder="Vermífugo" id="vermifugo">
    <input type="text" placeholder="Pulga/Carrapato" id="pulga">
    <input type="text" placeholder="Comportamento/Restrições" id="comportamento">
    <label>Castrado?</label>
    <select id="castrado">
      <option value="Sim">Sim</option>
      <option value="Não">Não</option>
    </select><br><br>
    <button type="submit">Salvar Pet</button>
  </form>

  <table id="tabelaPets">
    <thead>
      <tr>
        <th>Pet</th><th>Tutor</th><th>Porte</th><th>Espécie</th><th>Comida</th><th>Horário</th><th>Medicação</th><th>Vacinas</th><th>Vermífugo</th><th>Pulga/Carrapato</th><th>Comportamento</th><th>Castrado</th><th>Ações</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <!-- Registro de Creche -->
  <h2>Controle de Creche</h2>
  <form id="crecheForm">
    <input type="text" placeholder="Nome do Pet" id="crechePet" required>
    <input type="text" placeholder="Tutor" id="crecheTutor" required>
    <label>Porte:</label>
    <select id="crechePorte">
      <option value="P">P</option>
      <option value="M">M</option>
      <option value="G">G</option>
    </select>
    <label>Plano:</label>
    <select id="plano">
      <option value="Avulso">Avulso</option>
      <option value="Adaptação">Adaptação</option>
      <option value="1x">1x na semana</option>
      <option value="2x">2x na semana</option>
      <option value="3x">3x na semana</option>
      <option value="4x">4x na semana</option>
      <option value="5x">5x na semana</option>
      <option value="Personalizado">Valor Personalizado</option>
    </select>
    <input type="date" id="crecheData" required>
    <input type="number" placeholder="Valor personalizado" id="valorPersonalizado">
    <button type="submit">Registrar Frequência</button>
  </form>

  <table id="tabelaCreche">
    <thead>
      <tr>
        <th>Pet</th><th>Tutor</th><th>Porte</th><th>Plano</th><th>Data</th><th>Valor</th><th>Ações</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    let pets = JSON.parse(localStorage.getItem('pets')) || [];
    let creche = JSON.parse(localStorage.getItem('creche')) || [];

    document.getElementById('cadastroForm').onsubmit = e => {
      e.preventDefault();
      const pet = {
        nome: petNome.value,
        tutor: tutorNome.value,
        porte: porte.value,
        especie: especie.value,
        comida: comida.value,
        horario: horario.value,
        medicacao: medicacao.value,
        vacinas: vacinas.value,
        vermifugo: vermifugo.value,
        pulga: pulga.value,
        comportamento: comportamento.value,
        castrado: castrado.value
      };
      pets.push(pet);
      localStorage.setItem('pets', JSON.stringify(pets));
      renderPets();
      e.target.reset();
    };

    function renderPets() {
      const tbody = document.querySelector('#tabelaPets tbody');
      tbody.innerHTML = '';
      pets.forEach((p, i) => {
        tbody.innerHTML += `<tr>
          <td>${p.nome}</td><td>${p.tutor}</td><td>${p.porte}</td><td>${p.especie}</td>
          <td>${p.comida}</td><td>${p.horario}</td><td>${p.medicacao}</td><td>${p.vacinas}</td>
          <td>${p.vermifugo}</td><td>${p.pulga}</td><td>${p.comportamento}</td><td>${p.castrado}</td>
          <td><button onclick="excluirPet(${i})">Excluir</button></td>
        </tr>`;
      });
    }

    function excluirPet(i) {
      pets.splice(i, 1);
      localStorage.setItem('pets', JSON.stringify(pets));
      renderPets();
    }

    document.getElementById('crecheForm').onsubmit = e => {
      e.preventDefault();
      const plano = plano.value;
      let valor = 0;
      const valores = {
        'Avulso': 85,
        'Adaptação': 65,
        '1x': 79,
        '2x': 60,
        '3x': 55,
        '4x': 59,
        '5x': 45
      };
      if (plano === 'Personalizado') {
        valor = parseFloat(valorPersonalizado.value);
      } else {
        valor = valores[plano] || 0;
      }
      creche.push({
        pet: crechePet.value,
        tutor: crecheTutor.value,
        porte: crechePorte.value,
        plano,
        data: crecheData.value,
        valor
      });
      localStorage.setItem('creche', JSON.stringify(creche));
      renderCreche();
      e.target.reset();
    };

    function renderCreche() {
      const tbody = document.querySelector('#tabelaCreche tbody');
      tbody.innerHTML = '';
      creche.forEach((c, i) => {
        tbody.innerHTML += `<tr>
          <td>${c.pet}</td><td>${c.tutor}</td><td>${c.porte}</td><td>${c.plano}</td>
          <td>${c.data}</td><td>R$ ${c.valor.toFixed(2)}</td>
          <td><button onclick="excluirCreche(${i})">Excluir</button></td>
        </tr>`;
      });
    }

    function excluirCreche(i) {
      creche.splice(i, 1);
      localStorage.setItem('creche', JSON.stringify(creche));
      renderCreche();
    }

    renderPets();
    renderCreche();
  </script>
</body>
</html>
