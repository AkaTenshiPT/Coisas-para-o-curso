# Full-Stack App – To-Do & Notas

Pequena aplicação Flask + MySQL que permite gerir tarefas (_to-dos_) e **notas** (2.ª entidade), com animações suaves, fundo animado e uma rota extra `/random` que devolve um número de 1 a 100.

---

## 1. Instalação

```bash
git clone https://github.com/<teu-utilizador>/<repo>.git
cd <repo>

python -m venv venv          # cria ambiente virtual
# Windows:
venv\Scripts\activate
# Linux / WSL:
# source venv/bin/activate

pip install -r requirements.txt

## 2. Base de dados MySQL
CREATE DATABASE todo_app;
USE todo_app;
SOURCE sql/01_schema.sql;
Credenciais usadas em app.py

user      = todo_user
password  = pass_segura
database  = todo_app
(Se ainda não existir o utilizador:)

SQL
CREATE USER 'todo_user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pass_segura';
GRANT ALL PRIVILEGES ON todo_app.* TO 'todo_user'@'localhost';
FLUSH PRIVILEGES;


## 3. Executar
python app.py
# abre http://127.0.0.1:5000


## 4. Estrutura do projecto (resumida)
├── app.py                 # API + renderização
├── static/
│   ├── style.css          # estilos + animações
│   └── js/app.js          # lógica front-end
├── templates/
│   ├── index.html         # página principal
│   └── random.html        # rota /random
└── sql/
    └── 01_schema.sql      # tabelas todos + notes

## 5. Funcionalidades principais
Secção	Descrição
Tarefas	CRUD completo, botão ✓ alterna estado, caixote apaga
Notas	Segunda entidade (título + conteúdo) com CRUD e animação de entrada
/random	Página que mostra número aleatório (1-100) e botão “Voltar”
Efeitos visuais	Fundo que muda de cor + transições nas linhas adicionadas

## 6. Entidade Extra – Notas
## 6.1 Estrutura da tabela
sql
CREATE TABLE notes (
  id      INT AUTO_INCREMENT PRIMARY KEY,
  title   VARCHAR(255) NOT NULL,
  content TEXT         NOT NULL
);
(incluída no sql/01_schema.sql)

## 6.2 Rotas da API
Método	Endpoint	Função em app.py	Descrição
GET	/api/notes	list_notes()	Lista todas as notas
POST	/api/notes	add_note()	Cria nova nota
PUT	/api/notes/<id>	update_note(id)	Actualiza título / conteúdo
DELETE	/api/notes/<id>	delete_note(id)	Apaga nota

## 6.3 Exemplo rápido (cURL)
bash
# criar nota
curl -X POST http://127.0.0.1:5000/api/notes \
     -H "Content-Type: application/json" \
     -d '{"title":"Exemplo","content":"Primeira nota"}'

# listar notas
curl http://127.0.0.1:5000/api/notes

## 7. Alterações realizadas face ao template original
Ficheiro / pasta	Alteração
sql/01_schema.sql	Adicionada tabela notes
app.py	Rotas CRUD /api/notes, rota random_page() (template)
templates/index.html	Botão Número Aleatório, formulário de notas, <ul id="note-list">
templates/random.html	Novo — mostra número e botão voltar
static/js/app.js	Funções para notas (createNoteElement, renderNoteList, fetchNotes, addNote, deleteNote)
static/style.css	Fundo animado, transições opacity/translateY, layout flex, classes .btn & .note-row

## 8. Correspondência com o enunciado
Requisito	Implementação correspondente
Criar outra entidade funcional	Notas (notes) com CRUD completo
Incluir README descritivo	Este ficheiro
Dar mais efeitos visuais	Fundo animado + transições em tarefas/notas
Rota /random que devolve número 1-100	random_page() + botão de navegação