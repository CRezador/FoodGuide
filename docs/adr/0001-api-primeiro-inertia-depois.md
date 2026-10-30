# API REST primeiro, telas Inertia depois

O repositório nasceu do starter kit Laravel + Inertia/Vue, mas o desafio avalia uma API REST (Sanctum, `apiResource`, Resources, testes via Postman). Decidimos entregar primeiro a API completa em `routes/api.php`, com autenticação por token do Sanctum e respostas em JSON. As telas Inertia ficam como extensão opcional, construída depois por cima dos mesmos Policies, Form Requests e regras de domínio. Por isso, o starter kit fica sem uso até lá: não é código esquecido.
