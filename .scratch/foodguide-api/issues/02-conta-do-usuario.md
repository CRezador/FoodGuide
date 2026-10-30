Status: ready-for-agent
Blocked by: 01

# 02: Conta do Usuário: ver, editar, trocar senha, excluir

Spec: [../spec.md](../spec.md)

**O que construir:** o Usuário autenticado gerencia só a própria conta. Ele vê os próprios dados, edita nome e e-mail, troca a senha informando a atual e exclui a conta informando a senha. Não existem rotas para ver ou editar outros Usuários.

- [ ] `GET /api/user` devolve os dados do Usuário autenticado via o mesmo API Resource do ticket 01.
- [ ] `PUT/PATCH /api/user` edita nome e/ou e-mail. Um e-mail já usado por outra conta dá 422, e reenviar o próprio e-mail é aceito.
- [ ] A troca de senha pela mesma rota exige `current_password` correta e a nova senha confirmada, seguindo `Password::defaults()`. Sem a senha atual, ou com ela errada, a resposta é 422.
- [ ] `DELETE /api/user` exige `password` atual correta (sem ela, ou com ela errada, 422), apaga o Usuário e revoga todos os tokens dele (204).
- [ ] Depois de excluir a conta, o token antigo responde 401.
- [ ] A validação fica em Form Requests.
- [ ] Testes de Feature cobrem os itens acima pela API HTTP.
