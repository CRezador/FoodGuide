Status: ready-for-agent
Blocked by: 09

# 10: Relatório do Autor, com o ranking de Visualizações

Spec: [../spec.md](../spec.md)

**O que construir:** toda segunda-feira às 08:00, cada Autor cujas Receitas tiveram Visualizações na Semana de referência recebe o Relatório do Autor, com as 5 Receitas dele mais visualizadas, incluindo as privadas. Quem não teve atividade não recebe nada. O ranking de Curtidas chega no ticket 11.

- [ ] A consulta do ranking por Autor, separada da Notification:
  - reutiliza o cálculo da Semana de referência do ticket 09;
  - inclui Receitas privadas;
  - só inclui Receitas com contagem ≥ 1;
  - usa o mesmo desempate do ticket 09;
  - limita a 5 Receitas.
- [ ] Uma Notification por e-mail, enfileirada, lista título e número de Visualizações de cada Receita do Autor.
- [ ] Um comando Artisan envia a Notification só aos Autores com pelo menos uma Visualização na semana, cada um com o próprio ranking.
- [ ] O comando está agendado para toda segunda às 08:00.
- [ ] Testes do comando, com `Notification::fake()` e `travelTo`, cobrem:
  - cada Autor recebe só as próprias Receitas;
  - a ordem e o limite de 5;
  - as privadas incluídas;
  - os limites da semana;
  - Autor sem atividade não recebe;
  - Usuário sem Receitas não recebe.
- [ ] Um teste confere o agendamento.
