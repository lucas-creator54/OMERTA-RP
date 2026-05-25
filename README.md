<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GMOD Loading Screen</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

   body {
  overflow: hidden;
  background: #000;
  color: white;
}

.background {
  position: fixed;
  width: 100%;
  height: 100%;
  background-image: url("[./omerta_loading_screen/background.png](https://imgur.com/a/WkE5ya5)");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  filter: blur(4px) brightness(0.5);
  transform: scale(1.1);
}

    .overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 20px;
    }

    .server-title {
      font-size: 64px;
      font-weight: bold;
      text-transform: uppercase;
      letter-spacing: 5px;
      margin-bottom: 20px;
      text-shadow: 0 0 15px rgba(255,255,255,0.7);
    }

    .character-box {
      background: rgba(0,0,0,0.6);
      padding: 25px 50px;
      border-radius: 20px;
      border: 2px solid rgba(255,255,255,0.2);
      backdrop-filter: blur(10px);
      margin-bottom: 30px;
      box-shadow: 0 0 25px rgba(0,0,0,0.5);
    }

    .character-label {
      font-size: 20px;
      color: #bbbbbb;
      margin-bottom: 10px;
    }

    .character-name {
      margin-top: 20px;
      font-size: 42px;
      font-weight: bold;
      color: #00bfff;
      text-shadow: 0 0 15px rgba(0,191,255,0.8);
    }

    .input-field {
      width: 100%;
      padding: 14px;
      margin-top: 12px;
      border: none;
      border-radius: 12px;
      background: rgba(255,255,255,0.1);
      color: white;
      font-size: 16px;
      outline: none;
    }

    .input-field::placeholder {
      color: rgba(255,255,255,0.6);
    }

    .validate-btn {
      width: 100%;
      padding: 14px;
      margin-top: 15px;
      border: none;
      border-radius: 12px;
      background: linear-gradient(90deg, #00bfff, #0066ff);
      color: white;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      transition: 0.3s;
    }

    .validate-btn:hover {
      transform: scale(1.03);
    }

    .loading-bar {
      width: 500px;
      height: 22px;
      background: rgba(255,255,255,0.1);
      border-radius: 30px;
      overflow: hidden;
      margin-top: 20px;
      border: 1px solid rgba(255,255,255,0.2);
    }

    .loading-progress {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #00bfff, #0066ff);
      border-radius: 30px;
      animation: load 12s linear infinite;
    }

    @keyframes load {
      0% {
        width: 0%;
      }
      100% {
        width: 100%;
      }
    }

    .tips {
      position: absolute;
      bottom: 40px;
      font-size: 18px;
      opacity: 0.8;
    }

    .discord {
      position: absolute;
      top: 20px;
      right: 20px;
      background: rgba(0,0,0,0.5);
      padding: 10px 20px;
      border-radius: 12px;
      font-size: 16px;
    }
  </style>
</head>
<body>

  <div class="background"></div>

  <div class="overlay">
    <div class="server-title">Mon Serveur GMOD</div>

    <div class="character-box">
      <div class="character-label">Création du personnage</div>

      <input type="text" id="prenom" placeholder="Prénom" class="input-field" />
      <input type="text" id="nom" placeholder="Nom" class="input-field" />

      <select id="personnage" class="input-field">
        <option value="Citoyen">Citoyen</option>
        <option value="Policier">Policier</option>
        <option value="Médecin">Médecin</option>
        <option value="Gangster">Gangster</option>
      </select>

      <button onclick="validerPersonnage()" class="validate-btn">
        Valider le personnage
      </button>

      <div class="character-name" id="characterName">
        Aucun personnage sélectionné
      </div>
    </div>

    <div class="loading-bar">
      <div class="loading-progress"></div>
    </div>

    <div class="tips">
      Astuce : Respectez les règles RP pour une meilleure expérience.
    </div>

    <div class="discord">
      Discord : discord.gg/monserveur
    </div>
  </div>

  <script>
    function validerPersonnage() {
      const prenom = document.getElementById('prenom').value;
      const nom = document.getElementById('nom').value;
      const personnage = document.getElementById('personnage').value;

      if (!prenom || !nom) {
        alert('Veuillez entrer un prénom et un nom.');
        return;
      }

      document.getElementById('characterName').innerText =
        `${prenom} ${nom} • ${personnage}`;

      // Sauvegarde locale
      localStorage.setItem('prenom', prenom);
      localStorage.setItem('nom', nom);
      localStorage.setItem('personnage', personnage);
    }

    // Chargement automatique des anciennes données
    window.onload = () => {
      const prenom = localStorage.getItem('prenom');
      const nom = localStorage.getItem('nom');
      const personnage = localStorage.getItem('personnage');

      if (prenom) document.getElementById('prenom').value = prenom;
      if (nom) document.getElementById('nom').value = nom;
      if (personnage) document.getElementById('personnage').value = personnage;

      if (prenom && nom && personnage) {
        document.getElementById('characterName').innerText =
          `${prenom} ${nom} • ${personnage}`;
      }
    };

    // Liste d'astuces RP
    const astuces = [
      "Respectez les règles RP pour éviter les sanctions.",
      "Utilisez /report si vous avez un problème.",
      "Le FearRP est obligatoire.",
      "N'oubliez pas de rejoindre le Discord du serveur.",
      "Amusez-vous et restez fair-play !"
    ];

    let index = 0;
    const tips = document.querySelector('.tips');

    setInterval(() => {
      index = (index + 1) % astuces.length;
      tips.innerText = "Astuce : " + astuces[index];
    }, 4000);
  </script>

</body>
</html>
