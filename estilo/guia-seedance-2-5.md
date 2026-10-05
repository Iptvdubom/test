# Guia do Seedance 2.5 (baseado na documentação OFICIAL)

Fontes oficiais lidas por inteiro:
1. **Dreamina Seedance 2.5 prompt guide**, da BytePlus/ByteDance (atualizado em 28/09/2026): https://docs.byteplus.com/en/docs/ModelArk/2607689
2. **Skill oficial `sd25-pe`** (o otimizador de prompts da própria ByteDance, salvo em [`.claude/skills/sd25-pe/SKILL.md`](../.claude/skills/sd25-pe/SKILL.md))
3. **Anúncio oficial do Seedance 2.5** da ByteDance Seed: https://seed.bytedance.com/en/blog/one-take-creation-flexible-referencing-introducing-seedance-2-5

---

## 1. Limites

| Item | Valor oficial |
|---|---|
| Duração por geração | até **30 s** (configurada na ferramenta, **nunca** no texto) |
| Imagens de referência | até 30, cada uma até 4K |
| Vídeos de referência | até 10, **somando no máximo 30 s** |
| Áudios de referência | até 10, somando no máximo 30 s |
| Total de referências | até 50 |
| Idiomas de fala | mais de 10, gerados nativamente com sincronia labial (inclui português) |
| Vídeos longos | Por **extensões sucessivas** (Extend), mantendo personagem, cenário e ritmo. Dá pra chegar a vários minutos. |
| Melhor formato pra extensão | **MOV** (entrada e saída), pra manter cor, brilho e áudio contínuos |

**O que NÃO vai no prompt:** proporção (9:16), duração, resolução, fps e áudio ligado/desligado. Tudo isso se configura na tela.

---

## 2. Os 3 tipos de tarefa (escolha um por prompt)

| Tipo | Quando usamos | O que acontece |
|---|---|---|
| **Geração** | Bloco 1 e todo bloco que começa com **corte** pra outro lugar ou outra câmera | Você escolhe proporção e duração. |
| **Extensão** | Bloco que **continua a mesma cena** do bloco anterior | Continua o vídeo a partir do último quadro. A proporção fica travada na do vídeo original (deixe em *adaptive*); a duração você escolhe. |
| **Edição** | Consertar um detalhe de um bloco já gerado (uma fala, um objeto) | Mantém o vídeo e muda só o que você pedir. Funciona melhor com vídeos de até 20 s. |

Para uma extensão ser reconhecida, o prompt precisa da palavra-gatilho: **"Extend @Video1 forward"** ou "continue".

---

## 3. Estrutura oficial do prompt

O guia manda tratar o Seedance como **um produtor de vídeo** e escrever um roteiro estruturado. A estrutura da skill oficial tem estas seções:

```
【Generation Goal】        → em 1 frase: tipo de vídeo, quem é e o que acontece
【Reference Asset Roles】  → para que serve CADA imagem/vídeo enviado
【Subjects and Relationships】 → quantos personagens, o que é de quem, onde cada um fica
【Event Script】           → o acontecimento, por tempo ou por etapa, com estado inicial e final
【Maintain Consistency】   → o que não pode mudar
```

Regras de ouro:
- **Cada referência tem um papel explícito**, com número na ordem do upload: "Use @Image1 for the gorilla's face, fur and Hawaiian shirt; do not use the image background."
- **Não escreva o nome do personagem dentro da imagem** de referência. O vínculo é feito no texto.
- **Cada etapa ou intervalo tem uma única mudança principal** e termina num estado visível ("at the end, ...").
- **Prefira descrições positivas.** Use negativas só para legenda e áudio.
- **Emoção = ação visível:** "joga a cabeça pra trás e gargalha", e não "está muito feliz".
- **Ações:** descreva a maioria de forma geral e só detalhe as poucas ações memoráveis. Não repita a mesma ação.
- **Mostre a causa antes da reação:** primeiro o que aconteceu (a raiz soltou), depois a reação. Não feche no rosto antes de mostrar o motivo.

---

## 4. Marcação de tempo

- Use segundos inteiros e **sem buracos**: `0-5 seconds: ... 5-11 seconds: ... 11-18 seconds: ...`. Nada de "0-3s... 5-6s".
- Dá pra combinar com plano: `Shot 2 | 5-11s` (formato usado nos exemplos oficiais).
- Também dá pra marcar um ponto: "At the 5-second mark, hard cut to..."
- **Pouca coisa num intervalo:** o modelo improvisa. **Coisa demais:** ele corta demais ou pula partes. Distribua com bom senso.
- Não use tempo pra ações muito rápidas e repetidas ("balança a cabeça 3 vezes por segundo").
- Os intervalos são orçamentos de tempo para cada acontecimento, não cortes exatos no quadro.

---

## 5. Falas: o formato oficial

**A fala vai entre CHAVES `{ }`**, não entre aspas. Símbolos oficiais:

| Conteúdo | Símbolo | Exemplo |
|---|---|---|
| Fala | `{ }` | `{Óia isso, rapaz!}` |
| Efeito sonoro | `< >` | `<a raiz estala e solta>` |
| Música | `( )` | `(violão caipira ao fundo)`; nós não usamos música |
| Legenda | `【 】` | não usamos |

**Formato da fala:** `idioma + sotaque + jeito de falar + quem fala + {fala}`. A declaração vai **em cada fala**, não uma vez só no começo:

```
The gorilla says in Brazilian Portuguese with a rural caipira accent, laughing: {Ow! Saiu, rapaz!}
```

Se uma fala tem várias emoções, separe em linhas:
```
The gorilla's line (surprised): {Ow!}
The gorilla's line (laughing): {Saiu, rapaz!}
```

**⚠️ Para não aparecer legenda sozinha** (problema comum, segundo o guia oficial):
- **Não repita** palavras da fala depois dela. Ex.: não escreva `...{Óia isso}. He says "óia isso" with pride`.
- **Não coloque** instruções de tom presas a palavras específicas da fala. Use o formato `line (emoção): {fala}`.
- Se usar `< >` para efeitos, **não coloque nomes de personagens entre `< >`**.

---

## 6. Áudio sem música

O guia oficial avisa que, mesmo com "no BGM", o modelo às vezes transforma efeitos em música. O conserto oficial é:
1. Listar **todas** as palavras de música: *music, BGM, score, instrumental, melody, soundtrack, synth, ambient pad*.
2. Repetir essa regra no **começo e no fim** do prompt.

**⚠️ Água e eco:** citar água, ondas ou eco nas instruções de áudio pode gerar chiados, bolhas e ecos estranhos. Se a cena tem rio ou barco, mostre a água na imagem, mas **não cite água na parte de som**, ou cite o mínimo.

---

## 7. Problemas conhecidos (do FAQ oficial)

| Problema | Conserto oficial |
|---|---|
| **Olhos brilhando** (azul/vermelho) com emoções fortes | Troque palavras intensas ("extremamente chocado", "fanático") por neutras ("surpreso", "admirado") e adicione: *"his eyes stay normal and natural and never glow"*. Importante pro nosso gorila de olhos claros. |
| **Legenda aparece mesmo proibida** | Ver seção 5. |
| **Música aparece mesmo proibida** | Ver seção 6. |
| **Texturas de digital** na grama ou nas folhas | A imagem de referência não pode ser maior que a resolução de saída. Use `gorila-branco-1080p.jpg`. |
| **Erro HTTP 400** com JPG | Converta: `ffmpeg -i foto.jpg -pix_fmt yuvj420p foto-ok.jpg` (ou salve em PNG). |
| **Personagem trocado** com várias referências | Suba as imagens **na ordem em que os personagens aparecem** e numere no texto nessa ordem. |
| **Resultado instável** em tarefa complexa | Divida em tarefas menores (gere primeiro, edite depois). |
| **Áudio com bolhas ou eco** | Tire água, ondas e eco da descrição de som. |
| **Volume diferente** na extensão | É normal. Fica menor quando o vídeo original também foi gerado pelo 2.5. Ajuste no editor. |

---

## 8. Como fazer vídeos com mais de 30 s

Os nossos vídeos têm de 1:10 a 2:20, então cada vídeo vira **3 a 5 blocos de até 30 s**.

1. **Divida o roteiro em blocos** de 20 a 30 s que terminem num ponto limpo: depois de uma fala ou de uma reação. Nunca no meio de uma frase.
2. Para cada bloco, escolha:
   - **GERAÇÃO**, se o bloco começa com **corte** (outro lugar, outra câmera ou outro momento). Só a `@Image1` do personagem.
   - **EXTENSÃO**, se o bloco **continua a mesma cena** sem corte. Suba o bloco anterior como `@Video1` (em MOV, se a plataforma permitir) + `@Image1` do personagem.
3. Em **todo** bloco, repita a frase do `@Image1`, a descrição fixa, a voz e as regras de som e consistência.
4. Junte tudo no editor (CapCut ou outro).

---

## 9. Modelos prontos

### A) Bloco de GERAÇÃO

```
【Generation Goal】
Generate a funny, warm, photorealistic handheld vlog-style clip in which a huge albino gorilla <evento principal do bloco>. Audio policy: only the gorilla's spoken dialogue plus the specified sound effects and natural ambience; no music, BGM, score, instrumental, melody, soundtrack, synth or ambient pad at any moment.

【Reference Asset Roles】
Use @Image1 for the gorilla's face, cream pale-blonde fur, tall blonde crest, pinkish-beige skin, pale blue-gray eyes, heavy build and faded open navy-blue Hawaiian shirt with red hibiscus flowers; do not use the image background.

【Subjects and Relationships】
There is only one gorilla throughout the clip, and he always wears the open Hawaiian shirt defined by @Image1. <outros personagens, objetos e de quem é cada coisa>.

【Event Script】
0-6 seconds: <câmera/plano>. <estado inicial>; <ação principal>; at the end, <estado visível>. The gorilla says in Brazilian Portuguese with a rural caipira accent, <jeito>: {<fala>}
6-13 seconds: Hard cut to <nova câmera>. <...>. The gorilla's line (laughing): {<fala>}
13-20 seconds: <...>

【Visual Style and Camera】
Photorealistic smartphone vlog footage, natural light, realistic fur and skin detail, slight handheld shake when the gorilla holds the camera.

【Sound】
<efeito sonoro 1>, <efeito sonoro 2>. Ambience: <ambiente sem citar água>. Only the gorilla speaks. No music of any kind. No subtitles, captions or on-screen text.

【Maintain Consistency】
Keep the gorilla's identity, face, fur color, eye color and Hawaiian shirt identical throughout; his eyes stay normal and natural and never glow. Keep <objetos e cenário> consistent. The gorilla is always one single character, never duplicated. No music, BGM or score; no subtitles.
```

### B) Bloco de EXTENSÃO

```
@Video1 is the source video to extend forward.
Use @Image1 for the gorilla's face, cream pale-blonde fur, tall blonde crest, pinkish-beige skin, pale blue-gray eyes, heavy build and faded open navy-blue Hawaiian shirt with red hibiscus flowers; do not use the image background.

Extend @Video1 forward. The first frame of the extended segment directly continues from the final frame of @Video1: maintain continuity in the gorilla's posture and position, <objetos>, <lugar>, the camera position and framing, the lighting and the ambient sound. Audio policy: only the gorilla's dialogue, the specified sound effects and natural ambience; no music, BGM, score, instrumental, melody, soundtrack, synth or ambient pad.

【Stage 1】
Continuing from the final frame: <ação principal>; at the end, <estado visível>. The gorilla says in Brazilian Portuguese with a rural caipira accent, <jeito>: {<fala>}
【Stage 2】
<ação>; at the end, <estado>. The gorilla's line (<emoção>): {<fala>}
【Stage 3】
<ação final>; at the end, <estado final>.

【Sound】
<efeitos>. Ambience: <ambiente>. Only the gorilla speaks. No subtitles, captions or on-screen text.

Throughout the extension, keep the gorilla's identity, face, fur, eye color and Hawaiian shirt, <objetos>, the layout of <lugar> and the camera axis consistent; his eyes stay normal and never glow. The gorilla remains the same single continuous character throughout, never duplicated or split, and his body structure stays stable. No music, BGM or score; no subtitles.
```

**Configurações na tela:** GERAÇÃO → proporção 9:16, duração do bloco, áudio ligado. EXTENSÃO → proporção *adaptive* (travada), duração da extensão, áudio ligado e saída MOV se houver.
