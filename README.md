# LP Reduzida — Pedro Henrique Fonseca (link da bio)

Landing page enxuta do palestrante Pedro Henrique Fonseca (advogado tributarista, sócio e Head
of Tax Advocacy da PHF Tax): **hero → formulário**, mais página `/obrigado`.

Mesma estrutura da LP da Olívia Sensata, ou seja, o **padrão Polo de LP reduzida** (topbar
glass, hero em card escuro com foto e faixa de stats, seção de formulário em duas colunas,
`/obrigado` com saída no WhatsApp). O que muda são **as cores e as fontes**, tiradas da marca
dele (phftax.com.br):

| token | valor | origem |
|---|---|---|
| `--paper` | `#F7F5F1` | off-white quente do site dele |
| `--dark` | `#0D0D0F` | preto da marca, no card do hero |
| `--accent` | `#C21018` | vermelho escurecido para contraste em fundo claro |
| `--accent-bright` | `#FC594E` | vermelho da marca, usado sobre fundo escuro |
| `--amber` | `#FA6F1B` | laranja secundário, no card de destaque |

Tipografia: **Archivo Black** nos títulos (a palavra de impacto em vermelho, já que a fonte não
tem itálico) e **Hind** no texto, as duas fontes do site dele.

```
index.html      # topbar + hero (foto, card laranja, stats) + formulário leadlovers
obrigado.html   # confirmação + botão WhatsApp + rodapé Polo
css/styles.css  # tokens da marca dele sobre a estrutura Polo + override do form001
img/photos/     # hero.jpg, obrigado.jpg, pedro.jpg (avatar quadrado, reserva)
img/logos/      # polo.svg (branco, rodapé), polo-preto.svg (reserva p/ fundo claro)
assets/         # materiais-fonte e descritivo (fora do deploy e do git)
vercel.json     # deploy estático (cleanUrls)
```

Rodar local: `npx --yes serve . -l 4332`

## Pendências antes de publicar

1. **Formulário do leadlovers.** Falta só o `fid`: no `index.html` há dois `PEDRO_FID` para
   trocar pelo número do formulário do Pedro. Os ids dos campos já estão preenchidos
   (89892 e-mail, 89894 nome, 89893 telefone, 89891 texto), porque são os campos
   personalizados da conta da Polo e se repetem nas LPs da Olívia, do Murilo e do Faro.
   O `mid=681453` é a conta da Polo e não muda. Passo a passo abaixo.
2. **WhatsApp da `/obrigado`.** Está no número da Polo (`wa.me/5512982506250`) com mensagem
   pronta. Se quiser link rastreado por LP, gerar um `wa.link` como nas outras LPs e trocar.
3. **Fotos.** Trocadas em 2026-11-19 pelas da pasta do Drive enviada pela equipe (1024x1280,
   recorte 4:5): `hero.jpg` (retrato sorrindo) e `obrigado.jpg` (palestrando com microfone).
   Originais ficam em `assets/fotos-drive/` (fora do git e do deploy).

## Criar o formulário do Pedro no leadlovers (passo a passo)

1. Entrar no leadlovers e abrir o **Projeto** (máquina) onde ficam os formulários das LPs de
   palestrante, o mesmo da Olívia Sensata.
2. Abrir a aba **Formulários de Captura**. Dica: abrir antes o formulário da Olívia em
   **Usar** e deixar numa aba, para copiar as mesmas configurações.
3. Clicar no **+** e escolher **Formulário Dinâmico** (é o tipo que gera o `formType=dynamic`
   usado aqui).
4. Preencher:
   - **Nome do formulário**: `LP Pedro Henrique Fonseca`, só para identificar.
   - **Texto botão**: `Solicitar proposta` (o visual quem manda é o `css/styles.css`, mas
     convém deixar igual).
   - **Url de destino de captura**: a URL da LP publicada + `/obrigado`
     (ex.: `https://lp-pedrohenrique.vercel.app/obrigado`). É o que leva o lead para a página
     de obrigado. Enquanto a LP não estiver publicada, pode voltar aqui depois e preencher.
   - **Sequência de destino lead**: a mesma lista/funil usada nas outras LPs de palestrante.
   - **Notificar ao capturar lead**: ligar, se a equipe quiser o aviso por e-mail.
   - **Captcha**: deixar desligado, senão aparece um desafio em cima do nosso layout.
   - **Design do formulário**: deixar o padrão (`form001`), que é o que o CSS daqui sobrescreve.
5. Escolher os campos, na ordem: **e-mail**, **nome**, **telefone** e o campo de texto
   (`Por que esse palestrante?`). São campos que já existem na conta da Polo: não criar novos,
   senão os ids mudam e o CRM separa a mesma informação em campos diferentes.
6. **Salvar**.
7. No formulário recém-criado, clicar em **Usar**. Aparecem três blocos de código: CSS (head),
   código do formulário (HTML) e JS (body). Só interessa o do meio.
8. Copiar o **código do formulário** inteiro e mandar para o Claude, ou procurar na primeira
   linha `name="fid" value="XXXXX"` e trocar os dois `PEDRO_FID` do `index.html` por esse
   número.
9. Testar: rodar a LP local, enviar um lead de teste e conferir se ele aparece no leadlovers.

Deploy (Vercel): importar o repositório, Framework Preset `Other`, sem build command nem
output directory.

## Notas
- O `form001.css` do leadlovers estiliza por **id** (`#llCaptureForm`), então o reset de
  padding/largura em `css/styles.css` precisa da mesma especificidade
  (`.form-card #llCaptureForm.llform001`). Com seletor de classe o form sai desalinhado.
- O `capture.js` aplica máscara própria no telefone (`+55 (__) _____-____`), substituindo
  o placeholder: comportamento esperado.
- O card do hero usa o número de seguidores (+370 mil) no lugar da frase-assinatura que a LP
  da Olívia tem, porque não há citação verificada do Pedro para usar ali.
- O reveal on scroll está inline no `index.html` (não há pasta `js/`).
