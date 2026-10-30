Status: ready-for-agent
Blocked by: None (can start immediately)

# 01: Autenticação: cadastro, login e logout

Spec: [../spec.md](../spec.md)

**O que construir:** um visitante cria a conta e já recebe um token. Um Usuário entra com e-mail e senha e recebe um token novo, e sai revogando só o token do dispositivo atual. Qualquer rota protegida sem token válido responde 401. Este ticket também prepara a base da API: instala o Sanctum (dependência já aprovada), cria o arquivo de rotas de API com o prefixo `/api`, muda o fuso da aplicação para `America/Sao_Paulo` e define `Password::defaults()` no AppServiceProvider.

- [ ] `POST /api/register` valida nome, e-mail único e senha confirmada seguindo `Password::defaults()` (mínimo de 8 caracteres, letras e números), e devolve 201 com o Usuário e um token.
- [ ] A senha é guardada com hash (cast `hashed`) e nunca aparece em nenhuma resposta. O Usuário é formatado por um API Resource (`id`, `name`, `email`, `created_at`, `updated_at`).
- [ ] `POST /api/login` devolve o Usuário e um token novo. Credenciais inválidas dão 422 com mensagem genérica, sem revelar se o erro foi no e-mail ou na senha.
- [ ] Logins em dois dispositivos geram dois tokens válidos ao mesmo tempo.
- [ ] `POST /api/logout` revoga só o token usado na requisição (204), e o outro token continua funcionando.
- [ ] Rotas protegidas sem token respondem 401 em JSON.
- [ ] `app.timezone` é `America/Sao_Paulo`.
- [ ] Testes de Feature em Pest cobrem todos os itens acima pela API HTTP.
