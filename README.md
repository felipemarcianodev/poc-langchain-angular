# Curso LangChain — Setup do Ambiente

Passo a passo para configurar o ambiente do zero em uma máquina nova (Windows).

## 1. Instalar o Anaconda

```powershell
winget install --id Anaconda.Anaconda3 --silent --accept-package-agreements --accept-source-agreements
```

> Alternativa: baixar o instalador em https://www.anaconda.com/download

## 2. Inicializar o conda no PowerShell

```powershell
& "$env:USERPROFILE\anaconda3\Scripts\conda.exe" init powershell
```

**Feche e reabra o terminal** depois desse comando para o `conda` ficar disponível.

## 3. Aceitar os Termos de Serviço dos canais

As versões recentes do conda exigem isso antes de instalar qualquer pacote:

```powershell
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/msys2
```

## 4. Criar e ativar o ambiente do curso

```powershell
conda create -n langchain python=3.12 -y
conda activate langchain
```

> O nome do ambiente é `langchain`. Sempre ative-o antes de rodar os scripts do curso.

## 5. Instalar os pacotes

Com o ambiente `langchain` ativado:

```powershell
pip install langchain langchain-openai langchain-pinecone python-dotenv
```

| Pacote | Para quê |
|---|---|
| `langchain` | Framework principal (inclui `langchain-core`) |
| `langchain-openai` | Integração com os modelos da OpenAI |
| `langchain-pinecone` | Integração com o vector store Pinecone (inclui o SDK `pinecone`) |
| `python-dotenv` | Carrega as chaves de API do arquivo `.env` |

## 6. Configurar as chaves de API

Crie um arquivo `.env` na raiz do projeto (`C:\Projetos\curso-lang-chain\.env`) com este conteúdo (sem aspas):

```dotenv
OPENAI_API_KEY=sk-...
PINECONE_API_KEY=pcsk_...
```

- **OpenAI**: https://platform.openai.com/api-keys
- **Pinecone**: https://app.pinecone.io → API Keys

> ⚠️ Os nomes precisam ser exatamente `OPENAI_API_KEY` e `PINECONE_API_KEY` — é o que o LangChain procura automaticamente.
> ⚠️ Nunca commite o `.env` no git (adicione ao `.gitignore`).

No início dos scripts Python, carregue as chaves com:

```python
from dotenv import load_dotenv

load_dotenv()
```

## 7. Verificar se tudo funcionou

```powershell
conda activate langchain
python -c "import langchain, langchain_openai, langchain_pinecone; print('LangChain', langchain.__version__, 'OK')"
```

## 8. JupyterLab

O JupyterLab já vem instalado com o Anaconda — não precisa instalar nada. Neste projeto ele roda na **porta 8889** (a padrão 8888 fica livre para outros usos):

```powershell
jupyter lab --port 8889 --notebook-dir "C:\Projetos\poc-langchain-angular"
```

Depois acesse no navegador:

```
http://localhost:8889/lab
```

> No primeiro acesso o Jupyter pede um **token** — ele aparece no terminal junto com a URL completa (ex.: `http://localhost:8889/lab?token=...`). Copie e cole essa URL no navegador.

Para parar o servidor: `Ctrl+C` no terminal (duas vezes para pular a confirmação).

Comandos úteis:

```powershell
jupyter lab --version       # verificar a versão instalada
jupyter server list         # listar servidores em execução (com URL e token)
```

### Configuração global (padrão de trabalho)

As preferências globais ficam em `C:\Users\Administrador\.jupyter\jupyter_server_config.json` e valem para **qualquer** forma de abrir o JupyterLab (terminal ou Anaconda Navigator):

```json
{
  "ServerApp": {
    "root_dir": "C:\\Projetos"
  }
}
```

- **Pasta padrão `C:\Projetos`**: o JupyterLab sempre abre nessa pasta. Assim, um `jupyter lab` simples já cai no diretório de trabalho — sem precisar do `--notebook-dir`.
- **Sem senha fixa**: não há `hashed_password` configurado, então a autenticação volta ao padrão por **token** (a URL com `?token=...` aparece no terminal). Se quiser definir uma senha, rode `jupyter lab password`.

> O `--notebook-dir` na linha de comando tem prioridade sobre o `root_dir` do config. Por isso o comando da porta 8889 acima ainda abre direto em `poc-langchain-angular` quando você quiser focar neste subprojeto.

## 9. Anaconda Navigator

Interface gráfica do Anaconda para gerenciar ambientes e abrir ferramentas (JupyterLab, Spyder etc.). Duas formas de abrir:

**Pelo Menu Iniciar** (mais comum): pressione a tecla `Windows`, digite **"Anaconda Navigator"** e clique no ícone verde.

**Pelo terminal:**

```powershell
anaconda-navigator
```

> ⚠️ O Navigator demora para carregar na primeira vez (30s+ é normal).
> ⚠️ O JupyterLab aberto pelo Navigator usa a porta padrão **8888** — diferente da 8889 usada neste projeto. Os dois podem rodar ao mesmo tempo sem conflito.

## Extra: Python do sistema (fora do conda)

Além do Anaconda, esta máquina tem o **Python 3.13** standalone instalado via winget (fica em `%LOCALAPPDATA%\Programs\Python\Python313`). Para reinstalar em outro PC:

```powershell
winget install --id Python.Python.3.13 --silent --accept-package-agreements --accept-source-agreements
```

> Alternativa: baixar o instalador em https://www.python.org/downloads/ (marcar a opção **"Add python.exe to PATH"**).

**Feche e reabra o terminal** depois da instalação e verifique:

```powershell
python --version    # deve mostrar Python 3.13.x
py --list-paths     # lista as versões de Python instaladas e seus caminhos
```

> ⚠️ Esse Python é independente do ambiente conda `langchain`. Os scripts do curso devem rodar com o ambiente conda ativado — o Python do sistema serve para uso geral no Windows.

## Comandos do dia a dia

```powershell
conda activate langchain    # ativar o ambiente (sempre antes de trabalhar)
conda deactivate            # sair do ambiente
conda env list              # listar ambientes
pip list                    # ver pacotes instalados no ambiente ativo
```
