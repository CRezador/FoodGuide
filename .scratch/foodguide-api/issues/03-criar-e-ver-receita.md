Status: ready-for-agent
Blocked by: 01

# 03: Criar e ver Receita (ainda sem Ingredientes)

Spec: [../spec.md](../spec.md)

**O que construir:** um Autor cria uma Receita com título, descrição, Modo de preparo (lista ordenada de passos), tempo de preparo em minutos, rendimento em porções, Categoria, URL da imagem opcional e Visibilidade, que nasce pública. Qualquer Usuário autenticado vê uma Receita pública pelo id. Uma Receita privada só é vista pelo Autor; para os demais Usuários, ela responde 404. Os Ingredientes chegam no ticket 04.

- [ ] Existe a tabela `recipes` com FK para o Autor em cascata, passos em JSON, `preparation_time_minutes`, `servings`, `category`, `image_url` (nullable), `is_public` (padrão true) e timestamps. Há model, factory (com estado `private`) e a relação com o User.
- [ ] O enum Categoria tem chaves TitleCase em inglês e rótulos em português, como na spec, e um método de rótulo.
- [ ] As rotas de Receita são registradas com `apiResource`, por enquanto só `store` e `show`.
- [ ] `POST /api/recipes` valida via Form Request:
  - título obrigatório (máx. 255);
  - descrição obrigatória;
  - tempo e porções inteiros ≥ 1;
  - Categoria dentro do enum;
  - `steps` com pelo menos um item, cada um uma string não vazia;
  - `image_url` como URL válida;
  - `is_public` como boolean.

  Responde 201.
- [ ] O Autor é sempre o Usuário autenticado: um `user_id` enviado no corpo é ignorado.
- [ ] O API Resource da Receita devolve `id`, `title`, `description`, `steps`, `preparation_time_minutes`, `servings`, `category` (`{ value, label }`), `image_url`, `is_public`, `author` (`{ id, name }`), `created_at` e `updated_at`.
- [ ] A RecipePolicy `view` permite quando a Receita é pública ou o Usuário é o Autor. Para Receita privada de outra pessoa, nega como 404 (`denyAsNotFound`).
- [ ] Excluir a conta apaga as Receitas do Usuário.
- [ ] Testes de Feature cobrem criação, validação, Autor forçado, ver pública, ver a própria privada, 404 na privada alheia e a cascata na exclusão da conta.
