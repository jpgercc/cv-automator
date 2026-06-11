# Perfect CV

``` text
[Dados Base: JSON] ➔ [Orquestrador: Python] ➔ [IA: LLM API] ➔ [Renderizador: Typst] ➔ [PDF Perfeito para ATS]
``` 

## Stack
- Python 3.12
- Engine de Renderização: Typst (melhor para ATS)
- API de LLM: Groq ou Google AI Studio (API do Gemini 1.5 Flash).:
	- Use o modelo llama-3.1-8b-instant ou o meta-llama/llama-4-scout-17b-16e-instruct, que gerenciam bem o teto de tokens, e use uma técnica de limpeza de dados no Python antes de enviar o prompt (por exemplo, remova links, termos repetidos ou a seção de benefícios da vaga que não agrega valor ao currículo).

## RNFs:
- Tratamento de Rate Limiting (HTTP 429): O sistema deve implementar uma estratégia de backoff exponencial ou uma fila de espera caso o limite de requisições por minuto (RPM) do Groq seja atingido.
- Isolamento de Erros: A falha na leitura de uma vaga específica não pode interromper a execução do script para as próximas vagas da fila.

- Indexabilidade de Texto: O arquivo PDF final gerado (via Typst) deve conter texto 100% selecionável e pesquisável. É estritamente proibido rasterizar o texto ou gerar o currículo como uma imagem embutida no PDF.
- Estrutura Linear de Parsing: O layout do documento gerado deve respeitar a ordem de leitura linear (esquerda para direita, cima para baixo), garantindo que conversores de PDF para TXT (usados pelos ATSs) não embaralhem as colunas.

- Incorporação de Fontes (Font Embedding): Todas as fontes utilizadas no documento devem ser incorporadas diretamente no binário do PDF para evitar que o sistema do recrutador substitua os caracteres por símbolos ilegíveis.
- Consumo de Memória: O script deve rodar de forma leve, sem vazamento de memória, permitindo sua execução em ambientes limitados (como containers Docker pequenos ou instâncias locais básicas com menos de 512MB de RAM disponíveis para o processo).
- Isolamento de Ambiente: O projeto deve ser empacotado de forma que o ambiente de execução seja facilmente replicável, utilizando um gerenciador de ambiente virtual (venv) e um arquivo requirements.txt explícito.

- Independência de Plataforma: O script Python e a CLI do Typst devem rodar de forma idêntica e sem alterações de código em sistemas Linux, macOS e Windows. (Irá rodar em Linux Ubuntu 26 LTS)
- Configuração via Variáveis de Ambiente: Chaves de API (GROQ_API_KEY) e caminhos de diretórios não devem estar fixos no código (hardcoded), sendo extraídos estritamente de arquivos .env ou do ambiente do sistema.

-Proteção de Dados Sensíveis: O script local não deve salvar históricos de chaves de API em logs abertos.

- Privacidade do Perfil: Como os dados do seu Perfil Mestre contêm informações pessoais (endereço, telefone, histórico), o script deve interagir apenas com APIs cujo termo de uso garanta que os dados enviados no prompt não serão utilizados para treinamento de modelos públicos (o plano de API do Groq e do Google AI Studio cobrem essa proteção).

``` text
cv-automator/
│
├── .venv/                  # Ambiente virtual Python (ignorado no Git)
├── .env                    # Chaves de API (GROQ_API_KEY=gsk_...)
├── .gitignore              # Configuração para não subir lixo ou chaves para o GitHub
├── requirements.txt        # Dependências do projeto (groq, python-dotenv, etc.)
│
├── data/                   # CAMADA DE DADOS
│   └── perfil_mestre.json  # Seu histórico profissional completo e intocado
│
├── templates/              # CAMADA DE DESIGN (VISUAL)
│   ├── curriculo.typ       # Template principal em Typst (com as variáveis e o layout)
│   └── componentes.typ     # (Opcional) Funções de estilo reutilizáveis do Typst
│
├── src/                    # CAMADA DE LÓGICA (CÓDIGO)
│   ├── __init__.py
│   ├── main.py             # Orquestrador que roda o fluxo completo
│   ├── ia_client.py        # Módulo responsável pela comunicação e limpeza com o Groq
│   └── compiler.py         # Módulo que monta o comando CLI e chama o Typst
│
└── output/                 # CAMADA DE SAÍDA (GERADOS)
    ├── CV_Joao_DevOps.pdf  # Exemplo de PDF gerado para uma vaga específica
    └── dados_vaga_temp.json # Opcional: Log do JSON que a IA cuspiu para auditoria
```
