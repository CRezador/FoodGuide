Status: ready-for-agent
Blocked by: 07, 10

# 11: Relatório do Autor, com o ranking de Curtidas

Spec: [../spec.md](../spec.md)

**O que construir:** o Relatório do Autor ganha a segunda seção: as 5 Receitas do Autor que receberam mais Curtidas na Semana de referência. Um Autor que só teve Curtidas na semana, sem Visualizações, passa a receber o Relatório. Uma seção sem atividade aparece com um aviso, em vez de sumir.

- [ ] A consulta do ranking de Curtidas conta as Curtidas criadas dentro da Semana de referência (não o total acumulado), inclui Receitas privadas, só inclui contagem ≥ 1, usa o mesmo desempate e limita a 5.
- [ ] A Notification do ticket 10 mostra as duas seções. Uma seção vazia mostra uma linha avisando que não houve atividade.
- [ ] O comando passa a enviar aos Autores com pelo menos uma Visualização **ou** Curtida na semana.
- [ ] Testes do comando cobrem:
  - a ordem e o limite do ranking de Curtidas;
  - Curtidas de fora da semana ignoradas;
  - o Autor só com Curtidas recebe;
  - a seção vazia com o aviso;
  - o Autor sem nenhuma atividade continua sem receber.
