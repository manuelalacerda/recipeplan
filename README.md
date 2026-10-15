# Recipe Plan 🍳

Planejador de receitas com Inteligência Artificial. O usuário digita o que gostaria de cozinhar e a aplicação gera uma receita completa, com título, descrição, ingredientes e modo de preparo.

Projeto desenvolvido na disciplina **Java Advanced** (2º semestre).

## Como funciona

1. O usuário digita um prato (por exemplo, "ovo frito") no campo de mensagem.
2. A `HomeView` envia o texto para o `RecipeService`.
3. O `RecipeService` chama o modelo de IA pelo Spring AI (`ChatClient`).
4. A resposta é convertida diretamente em um objeto `Recipe`.
5. A receita é exibida na tela pela `RecipeView`, dentro do `responsePanel`.

## Tecnologias

- Java 17
- Spring Boot 3.5
- Vaadin 24 (interface web em Java)
- Spring AI 1.0.3 (integração com a API da OpenAI)
- Gradle
- Lombok

## Estrutura do projeto

```
src/main/java/br/com/fiap/recipeplan
├── RecipeplanApplication.java    # Classe principal
├── model
│   ├── Recipe.java               # Receita (nome, descrição, ingredientes, instruções)
│   └── Ingredient.java           # Ingrediente (nome, quantidade, unidade)
├── service
│   └── RecipeService.java        # Solicita a receita à IA e retorna um Recipe
└── views
    ├── HomeView.java             # Tela inicial com o campo de mensagem
    └── RecipeView.java           # Componente que exibe a receita
```

## Pré-requisitos

- JDK 17 ou superior
- Uma chave de API da OpenAI (ou de um provedor compatível, como o Groq)

## Configuração

A chave da API é lida da variável de ambiente `OPENAI_API_KEY`, definida em `src/main/resources/application.properties`:

```properties
vaadin.launch-browser=true
spring.application.name=recipeplan

spring.ai.openai.api-key=${OPENAI_API_KEY}
```

> ⚠️ **Nunca escreva a chave diretamente no arquivo nem faça commit dela no repositório.**

### Definindo a variável de ambiente

**Linux / macOS**
```bash
export OPENAI_API_KEY=sua-chave-aqui
```

**Windows (PowerShell)**
```powershell
$env:OPENAI_API_KEY="sua-chave-aqui"
```

**IntelliJ IDEA**

Em *Run > Edit Configurations > Modify options > Environment variables*, adicione `OPENAI_API_KEY=sua-chave-aqui`.

### Usando outro provedor (opcional)

Para usar o Groq, que tem API compatível com a da OpenAI, ajuste o `application.properties`:

```properties
spring.ai.openai.api-key=${GROQ_API_KEY}
spring.ai.openai.chat.base-url=https://api.groq.com/openai/
spring.ai.openai.chat.options.model=openai/gpt-oss-20b
```

## Como executar

```bash
git clone https://github.com/<seu-usuario>/recipe-plan.git
cd recipe-plan
./gradlew bootRun
```

No Windows, use `.\gradlew.bat bootRun`.

Depois acesse [http://localhost:8080](http://localhost:8080). O navegador também abre sozinho ao iniciar a aplicação.

## Solução de problemas

| Problema | Causa provável | Solução |
|---|---|---|
| `Command line is too long` no IntelliJ | Classpath muito grande | Em *Modify options*, marque **Shorten command line** e escolha **JAR manifest** |
| Erro de placeholder `OPENAI_API_KEY` ao iniciar | Variável de ambiente não definida | Defina a variável (veja a seção de configuração) |
| Notificação "Não foi possível gerar a receita" | Chave inválida, sem crédito ou modelo sem acesso | Confira a chave e o saldo da conta |
