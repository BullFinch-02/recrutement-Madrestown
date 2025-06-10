<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Recrutement Ville Pixelmon</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600&display=swap');

    body {
      font-family: 'Montserrat', Arial, sans-serif;
      background-color: #121212;
      color: #e0e0e0;
      margin: 0;
      padding: 40px 20px;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
    }

    form {
      background: #1e1e1e;
      padding: 30px 40px;
      border-radius: 15px;
      max-width: 700px;
      width: 100%;
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.8);
      transition: box-shadow 0.3s ease;
    }
    form:hover {
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.9);
    }

    h2 {
      margin-bottom: 25px;
      color: #56ccf2;
      font-weight: 600;
      font-size: 1.9rem;
      text-align: center;
    }

    label {
      display: block;
      font-weight: 600;
      margin-bottom: 8px;
      margin-top: 25px;
      color: #a0a0a0;
      font-size: 1rem;
    }

    input[type="text"], input[type="number"], textarea {
      width: 100%;
      padding: 12px 15px;
      border-radius: 12px;
      border: 1.5px solid #333;
      font-size: 1rem;
      font-family: 'Montserrat', Arial, sans-serif;
      resize: vertical;
      background-color: #2a2a2a;
      color: #e0e0e0;
      transition: border-color 0.3s ease, background-color 0.3s ease;
      box-sizing: border-box;
    }

    input[type="text"]::placeholder,
    input[type="number"]::placeholder,
    textarea::placeholder {
      color: #7a7a7a;
    }

    input[type="text"]:focus, input[type="number"]:focus, textarea:focus {
      border-color: #56ccf2;
      outline: none;
      box-shadow: 0 0 8px rgba(86, 204, 242, 0.7);
      background-color: #3a3a3a;
    }

    textarea {
      min-height: 100px;
    }

    button {
      margin-top: 30px;
      width: 100%;
      background: #56ccf2;
      color: #121212;
      font-weight: 700;
      font-size: 1.1rem;
      padding: 14px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      transition: background 0.3s ease;
    }
    button:hover {
      background: #3498db;
      color: #fff;
    }

    #result {
      margin-top: 20px;
      font-weight: 600;
      font-size: 1rem;
      text-align: center;
      min-height: 24px;
      color: #27ae60;
    }

    #result.error {
      color: #e74c3c;
    }
  </style>
</head>
<body>
  <form id="pixelmon-form" autocomplete="off">
    <h2>Formulaire de Recrutement - Ville Pixelmon</h2>
    <div id="questions-container"></div>
    <button type="submit">Envoyer</button>
    <p id="result"></p>
  </form>

  <script>
    const questions = [
      { type: "text", label: "Pseudo Discord :", name: "discord_pseudo", placeholder: "Ex: MonPseudo#1234", required: true },
      { type: "text", label: "Nom du joueur dans le jeu :", name: "jeu_pseudo", placeholder: "Ton pseudo Minecraft / Pixelmon", required: true },
      { type: "number", label: "Âge :", name: "age", min: 10, max: 100, placeholder: "Ton âge", required: true },
      { type: "textarea", label: "Quelle est ta disponibilité (jours/horaires) ?", name: "disponibilite", placeholder: "Ex : Tous les soirs à partir de 18h", required: true },
      { type: "textarea", label: "Pourquoi veux-tu rejoindre notre ville ?", name: "q0", required: true },
      { type: "textarea", label: "Qu'est-ce qui t'a attiré dans notre communauté ?", name: "q1", required: true },
      { type: "textarea", label: "As-tu déjà été dans une autre ville avant ? Si oui, pourquoi l'as-tu quittée ?", name: "q2", required: true },
      { type: "textarea", label: "Depuis combien de temps joues-tu à Pixelmon / Minecraft ?", name: "q3", required: true },
      { type: "textarea", label: "Quel est ton niveau en construction, élevage de Pokémon ou farming ?", name: "q4", required: true },
      { type: "textarea", label: "As-tu déjà participé à des projets communautaires dans d'autres serveurs ?", name: "q5", required: true },
      { type: "textarea", label: "Qu'aimerais-tu apporter à la ville ?", name: "q6", required: true },
      { type: "textarea", label: "As-tu une spécialité ou un rôle que tu aimerais prendre dans la ville ?", name: "q7", required: true },
      { type: "textarea", label: "Combien de temps es-tu actif en moyenne par semaine ?", name: "q8", required: true },
      { type: "textarea", label: "Comment réagis-tu en cas de conflit avec un autre joueur ?", name: "q9", required: true },
      { type: "textarea", label: "Quelle est ta définition d'une bonne ambiance dans une communauté ?", name: "q10", required: true },
      { type: "textarea", label: "Es-tu d'accord avec les règles de la ville ?", name: "q11", required: true },
      { type: "textarea", label: "Es-tu d'accord pour respecter l'organisation de la ville ?", name: "q12", required: true },
      { type: "textarea", label: "Si tu ne peux plus jouer pendant une période, serais-tu à l'aise de prévenir un responsable ?", name: "q13", required: true },
      { type: "textarea", label: "As-tu lu et accepté les règles du serveur Poke Legends ?", name: "q14", required: true }
    ];

    const container = document.getElementById("questions-container");

    questions.forEach((q) => {
      const label = document.createElement("label");
      label.textContent = q.label;
      label.htmlFor = q.name;
      container.appendChild(label);

      let input;
      if (q.type === "textarea") {
        input = document.createElement("textarea");
        input.rows = 4;
        input.placeholder = q.placeholder || "";
      } else if (q.type === "number") {
        input = document.createElement("input");
        input.type = "number";
        if (q.min !== undefined) input.min = q.min;
        if (q.max !== undefined) input.max = q.max;
        input.placeholder = q.placeholder || "";
      } else if (q.type === "text") {
        input = document.createElement("input");
        input.type = "text";
        input.placeholder = q.placeholder || "";
      }
      input.name = q.name;
      if (q.required) input.required = true;
      input.id = q.name;
      container.appendChild(input);
    });

    document.getElementById("pixelmon-form").addEventListener("submit", async (e) => {
      e.preventDefault();
      const form = new FormData(e.target);

      const now = new Date();
      const date = now.toLocaleDateString("fr-FR");
      const time = now.toLocaleTimeString("fr-FR");

      // Construire le message complet pour la candidature
      let content = `**Nouvelle candidature Pixelmon**\nEnvoyée le **${date} à ${time}**\n\n`;

      questions.forEach((q) => {
        const answer = form.get(q.name) || "Non répondu";
        content += `> **${q.label}**\n\`\`\`\n${answer}\n\`\`\`\n`;
      });

      // Récupérer pseudo discord et pseudo jeu pour l'historique
      const discordPseudo = form.get("discord_pseudo") || "Inconnu";
      const jeuPseudo = form.get("jeu_pseudo") || "Inconnu";

      // Message historique court
      const historiqueContent = `**Nouvelle candidature reçue**\n> Pseudo Discord : **${discordPseudo}**\n> Pseudo en jeu : **${jeuPseudo}**\n> Date : ${date} à ${time}`;

      // Tes URLs webhook ici (remplace par les tiennes)
      const webhookCandidature = "https://discord.com/api/webhooks/1378452987617214514/j3Y6bkCZEkWWRaFFo91DSrNiG6ufRaCbHRajY5zSmkR6F--HPrbZoc6E_0wZ0RaJ7YKl";
      const webhookHistorique = "https://discord.com/api/webhooks/1378466946365915146/ydgN4WMc3o6lbK4-p7ZsaXpSL6zME0QUmZllW31EATbjdCDFhWWHRNGSOVm_2-2ruMLn";

      const resultElem = document.getElementById("result");

      try {
        // Envoi candidature complète
        let res1 = await fetch(webhookCandidature, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ content }),
        });
        if (!res1.ok) throw new Error("Erreur envoi candidature");

        // Envoi message historique
        let res2 = await fetch(webhookHistorique, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ content: historiqueContent }),
        });
        if (!res2.ok) throw new Error("Erreur envoi historique");

        resultElem.textContent = "✅ Réponses envoyées avec succès sur Discord !";
        resultElem.classList.remove("error");
        e.target.reset();
      } catch (err) {
        resultElem.textContent = "❌ Une erreur s'est produite lors de l'envoi.";
        resultElem.classList.add("error");
        console.error(err);
      }
    });
  </script>
</body>
</html>
