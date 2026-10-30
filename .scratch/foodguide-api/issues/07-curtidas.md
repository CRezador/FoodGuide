Status: ready-for-agent
Blocked by: 03

# 07: Curtidas

Spec: [../spec.md](../spec.md)

**O que construir:** um Usuário curte e descurta Receitas públicas de outras pessoas. Curtir duas vezes não duplica a Curtida, e descurtir algo que não foi curtido não dá erro. O Autor não pode curtir as próprias Receitas. A resposta da Receita mostra quantas Curtidas ela tem e se o Usuário atual já curtiu.

- [ ] Existe a tabela `recipe_likes` com FKs para Usuário e Receita em cascata, índice único (Usuário, Receita) e timestamps. Há model e factory.
- [ ] `POST /api/recipes/{recipe}/like` cria a Curtida e é idempotente: uma segunda chamada não cria registro novo e devolve sucesso.
- [ ] `DELETE /api/recipes/{recipe}/like` remove a Curtida e também é idempotente (204).
- [ ] A RecipePolicy `like` permite só se a Receita é pública e o Usuário não é o Autor. Curtir a própria Receita dá 403, e curtir uma privada alheia dá 404.
- [ ] O API Resource da Receita inclui `likes_count` e `liked_by_me`, também nas listagens, sem N+1.
- [ ] Excluir a conta apaga as Curtidas que o Usuário deu, e excluir a Receita apaga as Curtidas dela.
- [ ] Testes de Feature cobrem todos os itens acima pela API HTTP.
