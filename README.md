# Diretório de Contato — Câmara dos Deputados

Página HTML única (`diretorio_deputados.html`), sem instalação e sem servidor, para filtrar os 513 deputados federais e gerar e-mails pré-preenchidos para os selecionados.

Abra o arquivo dando duplo clique ou arrastando para o navegador (Chrome, Firefox, Edge, Safari). Funciona offline — todos os dados já estão embutidos no próprio arquivo.

---

## O que a página faz

- Lista os deputados com nome, partido, estado (UF) e espectro político.
- Filtra por:
  - **Espectro político** (chips: Esquerda, Centro-esquerda, Centro, Centro-direita, Direita, Sem classificação) — pode marcar mais de um ao mesmo tempo.
  - **Estado (UF)** — lista suspensa.
  - **Partido** — lista suspensa.
  - **Busca por nome** — campo de texto livre.
- Permite **selecionar deputados individualmente** (caixinha no canto de cada card) ou **selecionar todos os que estão visíveis** no filtro atual.
- A partir da seleção, oferece três ações:
  | Botão | O que faz |
  |---|---|
  | **Copiar e-mails** | Copia a lista de e-mails (selecionados, ou todos os filtrados se nada estiver marcado) para a área de transferência. |
  | **Baixar .txt** | Gera um arquivo `.txt` com um e-mail por linha. |
  | **Gerar links de e-mail** | Abre um painel com um botão **"Abrir e-mail"** para cada deputado selecionado. Cada botão usa o link `mailto:`, que abre o programa de e-mail padrão configurado no computador (Outlook, Apple Mail, Thunderbird, ou o Gmail se ele estiver definido como padrão no navegador), já com o **assunto** e o **corpo** preenchidos a partir do modelo configurado no código (ver seção abaixo). |

---

## Como editar o modelo (template) do e-mail

Dentro do arquivo `diretorio_deputados.html`, próximo ao início da seção `<script>`, há este bloco bem identificado:

```javascript
const EMAIL_TEMPLATE = {
  // <<< COLOQUE AQUI O TÍTULO (ASSUNTO) DO E-MAIL >>>
  assunto: "TÍTULO DO E-MAIL AQUI",

  // <<< COLOQUE AQUI O CORPO/TEXTO DO E-MAIL >>>
  corpo:
`Excelentíssimo(a) Deputado(a) {{nome}},

CORPO DO E-MAIL AQUI.

Atenciosamente,
SEU NOME AQUI`
};
```

**Passo a passo:**
1. Abra o arquivo `.html` em um editor de texto simples (Bloco de Notas, VS Code, TextEdit, Notepad++ etc. — não use o Word).
2. Use `Ctrl+F` (ou `Cmd+F`) e procure por `MODELO DE E-MAIL` para chegar direto nesse bloco.
3. Substitua `"TÍTULO DO E-MAIL AQUI"` pelo assunto desejado.
4. Substitua o conteúdo entre os acentos graves (`` ` ``) pelo corpo da mensagem.
5. Salve o arquivo (mantendo a extensão `.html`) e recarregue a página no navegador.

**Dicas:**
- O marcador `{{nome}}` é substituído automaticamente pelo nome do deputado ao gerar cada link — pode usá-lo tanto no assunto quanto no corpo.
- Quebras de linha no corpo (pressionar Enter dentro do texto entre os acentos graves) são respeitadas no e-mail final.
- Enquanto o texto ainda contiver `"TÍTULO DO E-MAIL AQUI"` ou `"CORPO DO E-MAIL AQUI"`, a página exibe um aviso no painel de links lembrando que o modelo ainda não foi personalizado.

---

## Limitação importante: isto não envia e-mails automaticamente

Esta página é um arquivo HTML estático, sem servidor por trás. Por isso, ela **não dispara e-mails de verdade por conta própria** — por segurança, nenhum site comum tem permissão para acessar sua conta de e-mail e enviar mensagens sem você confirmar.

O que a página faz é **preparar e abrir** um rascunho pronto (com destinatário, assunto e corpo já preenchidos) no aplicativo de e-mail que está configurado como padrão no seu computador ou navegador. Você ainda precisa clicar em **"Enviar"** dentro do seu programa de e-mail para cada um.

Por esse motivo, cada deputado selecionado gera **um link separado** — isso garante que cada e-mail saia endereçado individualmente, em vez de uma cópia genérica para vários destinatários ao mesmo tempo.

> Caso no futuro você queira enviar os e-mails automaticamente (sem precisar clicar em "Enviar" um por um), isso exige integrar um serviço externo de envio (como EmailJS, SendGrid, etc.), com cadastro e configuração de credenciais — é uma etapa adicional, fora do que um HTML simples consegue fazer por conta própria.

---

## Estrutura dos dados

Os dados (nome, partido, UF, espectro político e e-mail institucional) estão embutidos diretamente no arquivo, na constante `DATA`, no formato:

```javascript
{ "n": "Nome do Deputado", "p": "Partido", "e": "UF", "s": "Espectro", "m": "email@camara.leg.br" }
```

**Fonte:** Dados Abertos da Câmara dos Deputados. A classificação de espectro político é uma aproximação feita por partido e não reflete necessariamente o posicionamento individual declarado por cada parlamentar.
