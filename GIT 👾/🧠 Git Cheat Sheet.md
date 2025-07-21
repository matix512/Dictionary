

> Guia rápido com os comandos Git mais usados, organizado por categorias.

---

## 📸 STAGE & SNAPSHOT  

Trabalhar com snapshots e área de staging:

```bash
git status                    # Mostrar ficheiros modificados e staged
git add [ficheiro]           # Adicionar ficheiro ao próximo commit (staging)
git reset [ficheiro]         # Remover ficheiro da staging area (mantém as alterações)
git diff                     # Ver diferenças não staged
git diff --staged            # Ver diferenças staged mas não comitadas
git commit -m "mensagem"     # Criar um commit com as alterações staged
```


## 🛠️ SETUP

Configurar informações globais do utilizador:

```bash
git config --global user.name "Primeiro Último"
git config --global user.email "email@exemplo.com"
git config --global color.ui auto
```

## 🚀 SETUP & INIT

Inicializar e clonar repositórios:

```bash
git init                     # Inicializar repositório Git
git clone [url]             # Clonar um repositório remoto
```

## 🌿 BRANCH & MERGE

Isolar trabalho, mudar contexto e fundir alterações:

``` bash
git branch                   # Listar branches (a atual tem um *)
git branch [nome-branch]     # Criar nova branch
git checkout [branch]        # Mudar de branch
git merge [branch]           # Juntar branch à atual
git log                      # Ver histórico de commits
```

## 🔄 SHARE & UPDATE

Atualizar e sincronizar repositórios remotos:

```bash
git remote add [alias] [url]    # Adicionar repositório remoto
git fetch [alias]               # Buscar branches do remoto
git merge [alias]/[branch]      # Fundir branch remota com a atual
git push [alias] [branch]       # Enviar commits para o remoto
git pull                        # Buscar e fundir do remoto
```

## 🗂️ TRACKING PATH CHANGES

Gerir renomeações e remoções de ficheiros:

```bash
git rm [ficheiro]                      # Remover ficheiro e marcar para commit
git mv [caminho-antigo] [novo]         # Mover/renomear ficheiro e preparar para commit
git log --stat -M                      # Histórico com info sobre ficheiros movidos
```

## 🧳 TEMPORARY COMMITS

Guardar temporariamente alterações:

```bash
git stash               # Guardar alterações atuais (não comitadas)
git stash list          # Ver lista de stashes
git stash pop           # Aplicar e remover o último stash
git stash drop          # Apagar o último stash
```

## 📝 REWRITE HISTORY

Modificar histórico e redefinir:

```bash
git rebase [branch]             # Reaplicar commits sobre outra branch
git reset --hard [commit]       # Apagar staging/working area e voltar a commit anterior
```

## 🔍 INSPECT & COMPARE

Ver logs, diffs e comparar objetos:

```bash
git log                              # Histórico da branch atual
git log branchB..branchA             # Commits em A que não estão em B
git log --follow [ficheiro]         # Histórico de alterações de um ficheiro
git diff branchB...branchA          # Diferenças entre branches
git show [SHA]                      # Mostrar conteúdo de um objeto (commit, ficheiro, etc.)
```

## 🚫 IGNORING PATTERNS

Ignorar ficheiros que não deves comitar:

```bash
git config --global core.excludesfile [ficheiro]    # Ignorar global
```

