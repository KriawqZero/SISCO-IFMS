# SISCO — Sistema de Gerenciamento de Carga Horária

O SISCO é um sistema desenvolvido para gerenciar e contabilizar as horas complementares dos alunos do Instituto Federal de Mato Grosso do Sul (IFMS) - Campus Corumbá. 

Sendo um dos meus maiores e mais desafiadores projetos, ele nasceu como nosso Trabalho de Conclusão de Curso (TCC) do ensino médio técnico integrado. O objetivo era resolver um gargalo real e administrativo da instituição: a burocracia, a lentidão e a dificuldade no acompanhamento manual das horas extracurriculares exigidas para a formação dos estudantes.

O projeto foi apresentado em julho de 2025, avaliado com nota máxima (10) pela banca, e rapidamente passou da fase acadêmica para o uso no mundo real.

## Impacto e Uso em Produção

Hoje, o SISCO não é apenas um projeto de gaveta; ele está operando em produção no campus Corumbá. 

A implementação inicial foi feita com os alunos dos últimos semestres do curso de Informática, e o sistema já é utilizado ativamente por **mais de 200 alunos**, além de professores e do coordenador do curso responsável pela validação das horas. Há planos por parte da instituição de expandir o uso para o curso de Metalurgia (os dois únicos cursos técnicos da unidade), centralizando todo o processo no sistema.

## Autoria e Divisão

O trabalho foi realizado em dupla, com uma separação clara de responsabilidades:

- **Marcilio Ortiz**: Responsável por toda a parte técnica, arquitetura, banco de dados e desenvolvimento do código a partir do zero.
- **Davi Nunes**: Responsável pela redação da documentação acadêmica (parte burocrática) e design.

Todo o código foi escrito manualmente na época, sem o uso de ferramentas modernas de IA, exigindo um estudo denso da linguagem e do framework para erguer um sistema robusto o suficiente para o uso institucional.

## Estrutura e Funcionalidades

O sistema opera com dois domínios principais de usuários, isolando as permissões e o escopo de cada um:

**Perfil do Aluno**
- Envio de certificados em anexo para comprovação de atividades.
- Acompanhamento do status de avaliação (pendente, aprovado, recusado).
- Exportação de relatórios em planilha (Excel) das horas já validadas.

**Perfil do Professor / Coordenador**
- Avaliação e validação dos certificados submetidos pelos alunos.
- Gestão cadastral (Alunos, Professores, Turmas, Cursos e Categorias de atividades).
- Visualização do painel de notificações do sistema.

## Decisões Técnicas

A aplicação foi construída sobre o ecossistema Laravel, adotando a TALL stack (Tailwind, Alpine, Laravel, Livewire) para a interface.

- **Backend**: PHP 8.2 e Laravel 11.
- **Frontend**: Livewire 3 e Alpine.js para interatividade, estilizado com Tailwind CSS 4.
- **Banco de Dados**: MariaDB.
- **Padrão de Arquitetura**: A lógica de negócio foi extraída dos *Controllers* e organizada em *Services* (ex: `CertificadoExportService`, `AlunoService`), facilitando a manutenção e a leitura.
- **Tarefas em Segundo Plano**: Utilização de *Jobs* e filas do Laravel para não bloquear as requisições principais do usuário ao disparar rotinas pesadas.
- **Exportação**: Uso da biblioteca PhpSpreadsheet para a geração de relatórios de carga horária.

## Aprendizados

Como foi nosso TCC do ensino médio, o SISCO representou o meu primeiro contato com o ciclo de vida completo de um software: do levantamento de requisitos ao deploy em produção para centenas de usuários reais.

Além da pressão natural de entregar um trabalho de conclusão, os aprendizados foram profundos:
- **Design de Arquitetura:** A adoção do *Service Pattern* foi uma virada de chave. Isolar a lógica de validação e regras de negócio (`app/Http/Services`) impediu que a base de código virasse um caos de *Controllers* gigantes, um erro muito comum em primeiros projetos.
- **Escala e Vida Real:** Ver o sistema ser adotado por mais de 200 alunos e coordenadores ensina muito sobre responsabilidade. Lidar com sessões concorrentes, upload e armazenamento seguro de arquivos (certificados) e garantir que a interface fosse intuitiva para o usuário final foram desafios práticos que nenhum curso ensina.
- **Processamento Assíncrono:** Entender a necessidade de usar *Jobs* e filas para não travar o fluxo do sistema mostrou na prática a diferença entre um código que "funciona" e um código feito para um ambiente de produção.

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
