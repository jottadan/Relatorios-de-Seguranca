# Investigação — Campanha de Phishing no Roblox

## Contexto

Recentemente, começou a circular nas redes sociais um alerta afirmando
que usuários do Roblox poderiam perder suas contas simplesmente ao
aceitar solicitações de amizade de determinadas pessoas.

A investigação teve como objetivo verificar se o simples aceite de uma
solicitação de amizade poderia expor informações sensíveis ou permitir
o comprometimento da conta.

---

## Hipóteses

A hipótese inicial era que o sistema de amizades pudesse expor alguma
informação sensível da conta alvo, como:

* credenciais;
* tokens de autenticação;
* informações de sessão;
* ou outros dados que pudessem ser utilizados para assumir a conta.

Apesar de realmente haver informação sensível de uma conta em seu body request, foi analisado que isso só fica visível ao usuário que já está logado na conta.

Também foi considerada uma hipótese alternativa: a solicitação de
amizade poderia ser apenas o primeiro passo de uma campanha de
engenharia social, enquanto o comprometimento ocorreria posteriormente
por meio de phishing.

---

## Ambiente de teste

Foram criadas duas contas controladas:

* **Conta A** — utilizada para realizar as interações;
* **Conta B** — utilizada como conta alvo.

O tráfego de rede gerado pela Conta A foi analisado durante diferentes
interações com a Conta B.

---

## Análise do tráfego

Foram testadas as seguintes ações:

### Solicitação de amizade

A Conta A enviou uma solicitação de amizade para a Conta B e o tráfego
gerado pela ação foi analisado.

Foram visto somente informações já disponíveis pela própria plataforma, como ID de itens usados, assim como username e userID.

Não foi identificada nenhuma informação sensível pertencente à Conta B
que pudesse fornecer acesso à conta.

### Bloqueio e desbloqueio

A Conta A bloqueou e posteriormente desbloqueou a Conta B.

Novamente, não foi observada nenhuma informação capaz de permitir a
autenticação ou tomada de controle da Conta B.

### Chat

Foram realizadas interações pelo sistema de chat entre as duas contas.

O tráfego gerado também foi analisado em busca de credenciais, tokens
de autenticação ou outras informações sensíveis.

Nenhum dado desse tipo foi identificado.

---

## Investigação do possível phishing

Após os testes de tráfego, foi analisado um dos relatos que foi divulgado no TikTok, nele foi levantado a hipótese alternativa para explicar os relatos de contas comprometidas.

Foi possível reproduzir um cenário de phishing utilizando recursos
legítimos da plataforma como parte da engenharia social.

A cadeia observada pode ser representada da seguinte forma:

```text
Solicitação de amizade
↓
Vítima aceita
↓
Atacante estabelece contato
↓
Falso evento de moderação / expulsão do jogo
↓
Falsa tela de login
↓
Vítima fornece as credenciais
↓
Credenciais são enviadas para infraestrutura externa
↓
Possível comprometimento da conta
```

A reprodução desse cenário foi realizada com sucesso durante a
investigação. Veja **evidence.md**.

---

## Análise

A análise do tráfego não apresentou evidências de que o simples aceite
de uma solicitação de amizade comprometa diretamente a conta alvo.

Os testes realizados também não mostraram exposição de credenciais,
tokens ou outras informações que permitissem à Conta A assumir o
controle da Conta B.

Por outro lado, o cenário de phishing reproduzido apresenta uma
explicação plausível para os relatos de comprometimento.

Nesse caso, a solicitação de amizade pode funcionar apenas como o
primeiro ponto de contato com a vítima. Depois disso, o atacante utiliza
engenharia social para conduzi-la a uma situação falsa e convencê-la a
fornecer suas próprias credenciais.

Elementos presentes dentro das próprias "experiências", como situações de moderação ou negociações de itens, também podem ser utilizados para tornar o golpe mais convincente.

---

## Conclusão

A investigação não encontrou evidências de que aceitar uma solicitação
de amizade, por si só, exponha credenciais, tokens de autenticação ou
outras informações suficientes para comprometer uma conta.

O cenário reproduzido é mais compatível com uma campanha de **phishing**
**e engenharia social**, na qual a solicitação de amizade funciona como
um meio de aproximação da vítima e o roubo de credenciais ocorre em uma
etapa posterior.

Portanto, a afirmação de que **“aceitar a solicitação de amizade faz a**
**conta ser hackeada”, que está circulando pelas redes, não foi confirmada pelos testes realizados.**

---

## Limitações

A investigação foi realizada utilizando contas controladas e teve como
foco o comportamento observável pelo cliente.

Os resultados não permitem afirmar que todos os casos relatados de
comprometimento de contas do Roblox utilizem exatamente essa mesma
cadeia de ataque, nem descartam a existência de outras vulnerabilidades
ou técnicas de comprometimento.
