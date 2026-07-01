# SISCO — Sistema de Gerenciamento de Carga Horária

O SISCO é um sistema desenvolvido para gerenciar e contabilizar as horas complementares dos alunos do Instituto Federal de Mato Grosso do Sul (IFMS). 

Este projeto nasceu como nosso Trabalho de Conclusão de Curso (TCC) do ensino médio técnico integrado. Ele foi desenhado para resolver um problema real da instituição: a burocracia e a dificuldade no acompanhamento das horas extracurriculares exigidas para a formação dos estudantes.

O projeto foi apresentado em julho de 2025 e avaliado com nota máxima (10) pela banca.

## Autoria e Divisão

O trabalho foi realizado em dupla, com uma separação clara de responsabilidades:

- **Marcilio Ortiz**: Responsável por toda a parte técnica, arquitetura, banco de dados e desenvolvimento do código a partir do zero.
- **Davi Nunes**: Responsável pela redação da documentação acadêmica (parte burocrática) e design.

Todo o código foi escrito manualmente, sem o uso de ferramentas de IA modernas, exigindo um estudo aprofundado da linguagem e do framework para fazer o sistema funcionar de ponta a ponta.

## Estrutura e Funcionalidades

O sistema opera com dois domínios principais de usuários, isolando as permissões e o escopo de cada um:

**Perfil do Aluno**
- Envio de certificados em anexo para comprovação de atividades.
- Acompanhamento do status de avaliação (pendente, aprovado, recusado).
- Exportação de relatórios em planilha (Excel) das horas já validadas.

**Perfil do Professor / Coordenador**
- Avaliação dos certificados submetidos pelos alunos.
- Gestão cadastral (Alunos, Professores, Turmas, Cursos e Categorias de atividades).
- Visualização do painel de notificações do sistema.

## Decisões Técnicas

A aplicação foi construída sobre o ecossistema Laravel, adotando a TALL stack (Tailwind, Alpine, Laravel, Livewire) para a interface.

- **Backend**: PHP 8.2 e Laravel 11.
- **Frontend**: Livewire 3 e Alpine.js para interatividade, estilizado com Tailwind CSS 4.
- **Banco de Dados**: MariaDB.
- **Padrão de Arquitetura**: A lógica de negócio foi extraída dos *Controllers* e organizada em *Services* (ex: `CertificadoExportService`, `AlunoService`), facilitando a manutenção e a leitura.
- **Tarefas em Segundo Plano**: Utilização de *Jobs* e filas do Laravel para não bloquear as requisições principais do usuário (como envio de e-mails ou notificações).
- **Exportação**: Uso da biblioteca PhpSpreadsheet para a geração de relatórios de carga horária.

## Aprendizados

Por ser um projeto de ensino médio, o SISCO foi o primeiro contato com o desenvolvimento completo de uma aplicação do zero até a produção. 

O principal aprendizado técnico foi a adoção do Service Pattern. Ao invés de colocar toda a regra de validação e salvamento de certificados nos controllers, isolar essa lógica nos `Services` manteve o código organizado e fácil de debugar. Também foi necessário aprender a lidar com upload de arquivos reais, validação de sessões e a integração de processamento assíncrono usando Jobs, desafios comuns que não costumam aparecer em tutoriais simples.

## Como Executar Localmente

### Pré-requisitos
- PHP 8.2+
- Composer
- Node.js 18+
- MariaDB / MySQL
- Git

### Passos

1. Clone o repositório:
```bash
git clone https://github.com/KriawqZero/IFMS-Sistema_CargaHoraria.git
cd IFMS-Sistema_CargaHoraria
```

2. Instale as dependências:
```bash
composer install
npm install # ou yarn install / pnpm install
```

3. Configure o ambiente:
```bash
cp .env.example .env
php artisan key:generate
```
*Não se esqueça de preencher as credenciais do seu banco de dados no arquivo `.env`.*

4. Execute as migrações e popule o banco (seeding):
```bash
php artisan migrate:fresh --seed
```

5. Inicie os servidores de desenvolvimento:
```bash
# Em um terminal, inicie o backend:
php artisan serve

# Em outro terminal, inicie o build do frontend:
npm run dev
```

6. O sistema estará disponível em `http://localhost:8000`.
