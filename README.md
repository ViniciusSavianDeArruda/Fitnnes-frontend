# Fitnnes-frontend

![Next.js](https://img.shields.io/badge/next.js-15-blue)
![TypeScript](https://img.shields.io/badge/typescript-5-blue)
![Tailwind CSS](https://img.shields.io/badge/tailwindcss-4-teal)
![License](https://img.shields.io/badge/license-MIT-green)

## Demo

Aplicação Web  
https://app.fitnnesapp.online

API Backend  
https://api.fitnnesapp.online

Fitnnes-frontend é o webapp do sistema de gestão de treinos Fit.IA, desenvolvido com Next.js 15, TypeScript, Tailwind CSS e shadcn/ui. Este projeto consome a API backend, entrega a interface principal do produto, autenticação, visualização de treinos, estatísticas e integração com dispositivos.

---

## Features

- Google OAuth authentication (BetterAuth)
- Workout plan and day visualization
- Exercise tracking
- Consistency heatmap and streak stats
- Responsive mobile-first UI
- Integration with smartwatch
- Modular UI with shadcn/ui
- Form validation with Zod
- API client generation with Orval

---

## Tecnologias Utilizadas

- **Next.js 15**: Framework React para web apps modernos.
- **TypeScript**: Tipagem estática e segurança.
- **Tailwind CSS**: Estilização rápida e customizável.
- **shadcn/ui**: Componentes UI acessíveis e modernos.
- **React Hook Form + Zod**: Formulários e validação.
- **Orval**: Geração automática de client API.
- **dayjs**: Manipulação de datas.
- **BetterAuth**: Autenticação e gerenciamento de sessão.

---

## Estrutura do Projeto

```
Fitnnes-frontend/
├── app/
│   ├── _lib/           # API client, auth client, fetch mutator
│   ├── workout-plans/  # Páginas de planos e dias de treino
│   ├── stats/          # Página de estatísticas
│   ├── profile/        # Página de perfil
│   ├── auth/           # Página de login
│   └── globals.css     # Estilos globais
├── components/
│   └── ui/             # Componentes shadcn/ui customizados
├── public/             # Imagens e assets estáticos
├── tasks/              # Desafios e tarefas do bootcamp
├── README.md           # Documentação principal
```

---

## Como rodar

1. Instale dependências:
   ```bash
   pnpm install
   ```
2. Configure `.env`:
   ```env
   NEXT_PUBLIC_API_URL=http://localhost:8080
   NEXT_PUBLIC_BASE_URL=http://localhost:3000
   ```
3. Rode o servidor:
   ```bash
   pnpm dev
   ```

---

## Autenticação

O sistema utiliza autenticação baseada em sessão utilizando BetterAuth.

Fluxo:

1. Usuário faz login via Google OAuth
2. Backend gera sessão
3. Sessão é armazenada no banco
4. Frontend valida sessão em cada requisição

---

## Como contribuir

Contribuições são bem-vindas! Para colaborar:

1. Fork este repositório e crie uma branch para sua feature ou correção.
2. Siga o padrão de commits [Conventional Commits](https://www.conventionalcommits.org/).
3. Rode lint antes de abrir um PR:
   ```bash
   pnpm lint
   ```
4. Abra um Pull Request detalhando sua proposta.
5. Use issues para sugestões, bugs ou dúvidas.

### Padrões
- Código limpo, modular e tipado.
- Uso de componentes shadcn/ui.
- Validação com Zod.
- Formulários com React Hook Form.

---

## Roadmap

- [ ] Melhorar cobertura de testes automatizados
- [ ] Adicionar integração com notificações push
- [ ] Refatorar responsividade para tablets
- [ ] Evoluir integração com smartwatch
- [ ] Criar dashboard de estatísticas avançadas
- [ ] Adicionar temas customizáveis

---

## Licença

Este projeto está sob licença MIT. Veja o arquivo LICENSE para mais detalhes.

---

> Este projeto está em evolução contínua conforme vou estudando, aprendendo novas tecnologias e aprimorando boas práticas de desenvolvimento.


