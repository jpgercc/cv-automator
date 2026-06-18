# TO DO PLAN LIST

* A JSON could be set place in a new './config.json' file
at 'src/models.py'.
``` txt
class Curriculum(BaseModel):
    nome: str = Field(..., min_length=1)
    titulo: str = Field(..., min_length=1)
    email: str = Field(..., min_length=1)
    telefone: str = Field(..., min_length=1)
    linkedin: str = Field(default="")
    github: str = Field(default="")
    localizacao: str = Field(default="")
    resumo: str = Field(..., min_length=20)
    experiencias: list[Experience] = Field(..., min_length=1)
    habilidades: dict[str, Any] = Field(..., min_length=1)
    formacao: list[Education] = Field(..., min_length=1)
    certificacoes: list[str] = Field(default_factory=list)
    idiomas: list[Language] = Field(default_factory=list)
```

* 'config.json' should have:
- groq and gemni configs
- cv builder info and AI config (used in src/models.py, ai_client.py in _SYSTEM_PROMPT)
- cli configs (if any)

* Note: If it importes data from 'perfil_mestre.json' why not import date on the coluns to build the source code? So that json dictates for all that refer him?

* Instead of '/data' lets rename it '/config' and add 'config.json' there.

* Better noise removal in 'src/ai_client.py'

* Move functions and methods from 'main.py' to 'src/run.py'. In 'main.py' should only have self refer, like as:
```
# def main

if __name__ == "__main__":
    main()
```