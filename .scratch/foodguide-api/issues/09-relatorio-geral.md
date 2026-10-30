Status: ready-for-agent
Blocked by: 08

# 09: Relatório Geral

Spec: [../spec.md](../spec.md)

**O que construir:** toda segunda-feira às 08:00 (Brasília), todos os Usuários recebem por e-mail o mesmo Relatório Geral, com as 10 Receitas públicas mais visualizadas na Semana de referência (segunda a domingo da semana anterior). Se a semana não teve nenhuma Visualização, nada é enviado. O e-mail sai pelo mailer `log` em desenvolvimento.

- [ ] Existe um cálculo reutilizável da Semana de referência: dado "agora", devolve de segunda 00:00 a domingo 23:59:59 da semana anterior, em `America/Sao_Paulo`. O ticket 10 vai usar o mesmo cálculo.
- [ ] A consulta do ranking fica separada da Notification e:
  - considera só Receitas públicas no momento do envio;
  - conta as Visualizações com `viewed_on` dentro da semana;
  - só inclui Receitas com contagem ≥ 1;
  - ordena por contagem decrescente e, no empate, pela Receita criada mais recentemente;
  - limita a 10 Receitas.
- [ ] Uma Notification por e-mail, enfileirada (`ShouldQueue`, `MailMessage`), lista título, Autor e número de Visualizações de cada Receita.
- [ ] Um comando Artisan envia a Notification a todos os Usuários quando o ranking não está vazio.
- [ ] O comando está agendado em `routes/console.php` para toda segunda às 08:00.
- [ ] Testes do comando, com `Notification::fake()` e `travelTo`, cobrem:
  - todos os Usuários recebem;
  - a ordem e o limite de 10;
  - o desempate;
  - domingo 23:59 entra e a segunda seguinte não;
  - Receita privada ou excluída fica fora;
  - nada é enviado em semana vazia.
- [ ] Um teste confere o agendamento.
