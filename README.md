import express from "express";
import fetch from "node-fetch";

const app = express();
app.use(express.json());
app.use(require("cors")());

app.post("/chat", async (req, res) => {
  const pregunta = req.body.mensaje;

  const respuesta = await fetch("http://localhost:11434/api/generate", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "mistral",
      prompt: `
Eres un bot de SOPORTE NIVEL 0.
Solo orientas y das instrucciones básicas.
Si el problema es complejo, indica que contacte soporte humano.

Pregunta:
${pregunta}
`
    })
  });

  const data = await respuesta.json();
  res.json({ respuesta: data.response });
});

app.listen(3000, () => {
  console.log("Backend activo en puerto 3000");
});
