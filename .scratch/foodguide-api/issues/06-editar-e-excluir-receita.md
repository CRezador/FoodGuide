Status: ready-for-agent
Blocked by: 04

# 06: Editar e excluir Receita

Spec: [../spec.md](../spec.md)

**O que construir:** o Autor edita qualquer campo da própria Receita, inclusive a Visibilidade, enviando só os campos que quer mudar. Quando envia a lista de Ingredientes ou de passos, ela substitui a lista inteira. O Autor também pode excluir a Receita. Outros Usuários recebem 403 numa Receita pública e 404 numa privada.

- [ ] `PUT/PATCH /api/recipes/{recipe}` usa um Form Request com as mesmas regras da criação, mas com `sometimes`. Os campos omitidos não mudam.
- [ ] Se `ingredients` vier na requisição, os Ingredientes antigos são apagados e os novos gravados na ordem enviada, tudo de forma atômica. O mesmo vale para `steps`.
- [ ] `DELETE /api/recipes/{recipe}` exclui a Receita e os Ingredientes dela (204).
- [ ] A RecipePolicy `update`/`delete` permite só ao Autor. Para Receita pública de outra pessoa a resposta é 403; para privada de outra pessoa, 404.
- [ ] Mudar `is_public` para false tira a Receita da listagem pública.
- [ ] `updated_at` muda depois de uma edição.
- [ ] Testes de Feature cobrem a edição parcial, a substituição de Ingredientes e de passos, a validação, o 403 e o 404 de terceiros, a exclusão com cascata e a mudança de Visibilidade.
