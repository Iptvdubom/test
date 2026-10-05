# Guia do Seedance 2.5: como montar os prompts

Resumo das regras do Seedance 2.5 (lançado em 31/07/2026), compilado a partir do guia oficial de prompts da ByteDance/BytePlus e de guias que o reproduzem. As fontes estão no fim.

---

## 1. Limites e configurações

| Item | Valor | Onde se configura |
|---|---|---|
| Duração por geração | **4 a 30 s** em uma única passada | Controle de duração (não no texto) |
| Formato | 16:9, **9:16** (Reels/TikTok), 1:1, 4:3, 3:4, 21:9 | Controle de proporção |
| Resolução | 480p, 720p, 1080p (algumas plataformas chegam a 4K) | Controle de resolução |
| Referências | até **50**: 30 imagens, 10 vídeos e 10 áudios | Upload |
| Áudio | Gerado junto com o vídeo: fala com sincronia labial, efeitos e ambiente | Deixe o áudio **ligado** |
| Português | Suportado, com sincronia labial | Escrito no prompt |
| Estender vídeo | Continua um clipe a partir do **último quadro** (4–30 s a mais) | Função *Extend* |
| Quadro inicial/final | Dá pra fixar a imagem inicial e a final | Image-to-video |

> ⚠️ **Não escreva** duração, resolução, fps nem proporção dentro do prompt. Use os controles da ferramenta.

---

## 2. A fórmula oficial

**Sujeito + Ação/Acontecimento + Cenário + Estilo visual + Câmera/Cortes + Áudio**

O prompt funciona melhor como um **roteiro de produção compacto** do que como uma lista de adjetivos:
- Diga quem é e o que acontece de forma **visível**. Em vez de "ele está feliz", escreva "ele joga a cabeça pra trás e gargalha mostrando os dentes".
- Em cada movimento, diga **direção, distância, velocidade, contato e reação**.
- Em vídeos com mais de 15 s, divida em **etapas com tempo marcado**. Cada etapa tem estado inicial, acontecimento principal e estado final.
- Diga o que **não pode mudar** (rosto, camisa, cenário).

---

## 3. Marcação de tempo

Use **segundos inteiros**, cada intervalo em uma linha:

```
0-5s: ...
5-11s: ...
11-18s: ...
```

- **Se quiser cortes de câmera** (selfie → câmera fixa), escreva o corte dentro do intervalo: `11-18s: hard cut to a static tripod shot at chest height...`. O Seedance 2.5 organiza vários planos ligados dentro dos 30 s.
- **Se quiser plano-sequência** (sem corte), **não** use "Shot 1, Shot 2", porque isso faz o modelo cortar. Use só os intervalos e diga "one continuous take, no cuts".
- Uma ação principal por intervalo. Ações demais no mesmo trecho causam tremedeira e deformação.
- Não misture ordens que se anulam, como "câmera fixa" com "câmera girando rápido".

---

## 4. Referências (@image1, @video1, @audio1)

- Escreva em minúsculo e sem espaço: `@image1`, `@image2`, `@video1`, `@audio1`. O número segue a **ordem do upload**.
- **Toda referência precisa de uma frase dizendo para que serve**, e só para isso:
  `@image1 defines the main character's face, fur, body and Hawaiian shirt only; ignore its background.`
- Nunca escreva só "reference @image1" sem explicar o uso.
- Para continuidade, use o último quadro do clipe anterior como `@image2`:
  `@image2 is the last frame of the previous clip; start exactly from this pose, place and lighting.`

---

## 5. Fala (diálogo com sincronia labial)

- Diga o **idioma e o sotaque** antes da fala, depois diga **quem fala** e coloque a fala **entre aspas**, no idioma em que será falada.
  `Language: Brazilian Portuguese, rural caipira accent. The gorilla says, grinning at the camera: "Óia isso, meus fi!"`
- **Uma pessoa falando por vez** e **frases curtas**. Fala rápida, sotaque forte e música de fundo deixam a sincronia instável, então:
  - quebre as falas em frases curtas, uma por intervalo de tempo;
  - diga em que segundo a fala começa;
  - durante a fala, prefira o rosto visível (plano médio/fechado).
- Se a pronúncia sair errada, gere o trecho de novo ou corrija só aquele pedaço com a edição local.

---

## 6. Áudio em camadas

Separe sempre em 4 camadas:

```
Dialogue: (falas, com idioma e sotaque)
SFX: (efeitos: chiado, baque, crocância, água)
Ambience: (ambiente: mata, pássaros, vento, fogo)
Music: none (No BGM)
```

---

## 7. Proibições (negativas)

- Use negativas **com moderação**. Uma lista enorme compete com a ação que você quer.
- Para texto na tela, liste **os quatro juntos**, senão o que ficar de fora pode aparecer: `no subtitles, no text overlay, no watermark, no logo`.
- Diga a regra duas vezes: uma no trecho onde há fala e outra no fim do prompt. Ex.: `No subtitles, no BGM.`

---

## 8. Vídeos com mais de 30 s (como juntar vários prompts)

Os nossos vídeos têm de 1:10 a 2:20, então cada vídeo vira **3 a 5 blocos de até 30 s**.

1. **Divida o roteiro em blocos de 20 a 30 s** que terminem num **ponto limpo**: depois de uma fala, numa reação ou numa troca de lugar. Nunca corte no meio de uma frase.
2. **Bloco 1:** use `@image1` (personagem).
3. **Blocos seguintes**, de dois jeitos:
   - **A) Extend:** suba o bloco anterior como `@video1` e peça a continuação. O Seedance continua a partir do último quadro, mantendo personagem, luz e proporção. Melhor quando a cena continua no mesmo lugar.
   - **B) Novo clipe com continuidade:** `@image1` (personagem) + `@image2` (último quadro do bloco anterior). Melhor quando o bloco começa com um corte, num lugar novo ou com outra câmera.
4. **Repita em todo bloco**, sem mudar nada: a frase do `@image1`, a descrição visual fixa, a voz e o "Music: none".
5. Junte tudo no editor (CapCut ou outro).

---

## 9. Modelo de prompt (um bloco)

```
@image1 defines the main character's face, fur, body and Hawaiian shirt only; ignore its background.
[@image2 is the last frame of the previous clip; start exactly from this pose, place and lighting.]

Subject: [descrição visual fixa do personagem].
Scene: [lugar, hora do dia, luz, objetos importantes].
Style: photorealistic handheld vlog, natural light, realistic fur and skin detail, documentary feel.

0-6s: [câmera + ação visível + reação]. He says, [como]: "[fala curta]"
6-13s: [hard cut to ... / continua ...]. [ação]. He says: "[fala]"
13-20s: ...
20-27s: ...

Performance: playful, goofy and warm; big toothy grin; laughs at himself after every mishap; looks from the object to the camera to share the joke; heavy, clumsy body weight.
Dialogue: Language: Brazilian Portuguese, rural caipira accent, deep raspy warm male voice, one speaker at a time.
SFX: [efeitos].
Ambience: [ambiente].
Music: none.
Keep the character's face, fur and Hawaiian shirt identical throughout. No subtitles, no text overlay, no watermark, no logo, no BGM.
```

---

## Fontes

- [Dreamina Seedance 2.5 prompt guide — BytePlus ModelArk (oficial)](https://docs.byteplus.com/en/docs/ModelArk/2607689)
- [Introducing Seedance 2.5 — ByteDance Seed](https://seed.bytedance.com/en/blog/one-take-creation-flexible-referencing-introducing-seedance-2-5)
- [Seedance 2.5 guide — GitHub (gracech0322-cmd)](https://github.com/gracech0322-cmd/seedance-2-5)
- [How to Prompt Seedance 2.5 — Kapwing](https://www.kapwing.com/resources/how-to-prompt-seedance-2-5-a-guide-for-ai-video-creators/)
- [Seedance 2.5 Complete Guide — Luma AI](https://lumalabs.ai/learning-center/articles/seedance-2-5-complete-guide)
- [Multilingual video with Seedance 2.5 — Runware](https://runware.ai/docs/models/bytedance-seedance-2-5/guides/multilingual)
- [Seedance 2.5 Video Extend — ComfyUI](https://comfy.org/workflows/f3096e2ffb64-f3096e2ffb64/)
- [Seedance 2.5 Negative Prompts — Dreamina](https://dreamina.capcut.com/seedance/seedance-2-5-negative-prompts)
- [What Is Seedance 2.5? — MindStudio](https://www.mindstudio.ai/blog/what-is-seedance-2-5-bytedance-ai-video-model)
