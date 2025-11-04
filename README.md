Desafio full stack developer
Este projeto implementa um MVP (Produto Mínimo Viável) para o lançamento de notas de alunos com cálculo de média ponderada em tempo real, conforme solicitado no Desafio Técnico

O sistema é composto por uma API REST em Spring Boot (Java) e uma interface web reativa em Angular.ComponenteStack PrincipalBack-end (API)
Spring Boot 3 (Java 17), 
Spring Data JPA, H2 Database 4Front-end (UI)Angular 16+,
Reactive Forms 5, Angular Material 6IntegraçãoAPI REST, CORS configurado 

Escopo Funcional Implementado
O projeto cumpre o escopo de permitir o lançamento de notas por turma e disciplina, com as seguintes funcionalidades:

Seleção: Permite selecionar uma Turma e uma Disciplina via dropdowns.

Visualização: Exibe uma tabela editável com linhas = Alunos e colunas = Avaliações.

Média em Tempo Real: Calcula e exibe a média ponderada do aluno ((Σ nota × peso) / (Σ pesos)) imediatamente após a digitação da nota. Exibe "-" se não houver notas.

Pesos: Exibe o peso de cada avaliação (1 a 5) no cabeçalho da tabela.

Persistência: Salva todas as notas alteradas em lote no Back-end com um único clique (endpoint POST /salvar).

Validação: Notas entre 0 e 10 (validação no Front-end e Back-end)

Instruções de Execução
Você precisará ter o Java 17+ (ou superior) e o Node.js/npm instalados.

1. Back-end (Spring Boot API)
A API REST será executada na porta padrão 8080.

Passos:

Navegue até o diretório raiz do Back-end (ex: alunos-java-api).

Compile o projeto usando Maven ou Gradle:

Bash
# Para Maven (se for o caso)
./mvnw clean install
Inicie a aplicação:

Bash
# Para Maven
./mvnw spring-boot:run
Status Esperado: O console deve mostrar que o Spring Boot iniciou na porta 8080 e que os dados iniciais do data.sql foram carregados no banco H2 em memória.

2. Front-end (Angular UI)
A interface será executada na porta padrão 4200.

Passos:

Navegue até o diretório raiz do Front-end (ex: alunos-angular-ui).
Instale as dependências:

Bash
npm install
Inicie o servidor de desenvolvimento:

Bash
ng serve --open
Status Esperado: O navegador deve abrir automaticamente em http://localhost:4200/. Se houver algum erro de CORS, certifique-se de que o Back-end (porta 8080) esteja rodando.

Decisões Técnicas e Arquitetura
1. Arquitetura do Back-end (Java/Spring)
Camadas Clássicas (MVC): Utilização de Controller (para requisições), Service (para lógica de negócio e cálculo de média) e Repository (para acesso a dados via JPA). Esta estrutura garante organização e escalabilidade.

Inicialização de Dados (data.sql): O banco de dados H2 é populado automaticamente na inicialização com dados de teste para Turmas, Disciplinas, Alunos e Avaliações.

Tratamento de CORS: Configuração explícita (via @CrossOrigin ou CorsConfig) para permitir acesso do Front-end em http://localhost:4200, resolvendo o erro de origem cruzada.

Teste Unitário: Incluído BoletimServiceTest para validar a regra de negócio central: o cálculo da média ponderada.

Arquitetura do Front-end (Angular)
Reactive Forms: Utilizado para gerenciar a tabela de notas como um formulário dinâmico. Essa abordagem oferece controle robusto sobre a validação (0 a 10) e facilita a observação de mudanças.

Cálculo Reativo: O coração da aplicação é a observação do boletimForm.valueChanges. Um debounce time é aplicado para evitar o recálculo a cada tecla pressionada, otimizando o desempenho e garantindo o cálculo em tempo real.

Angular Material: Usado para construir uma interface coesa e profissional, utilizando componentes como mat-card, mat-select e mat-table.

Serviço Dedicado (BoletimService): Centraliza toda a lógica de comunicação com a API REST, mantendo o BoletimComponent focado apenas na interação com a interface (separação de responsabilidades)


Acesso à Documentação (Swagger/OpenAPI)Após iniciar o Back-end, você pode acessar a documentação interativa da API para testar os endpoints diretamente:http://localhost:8080/swagger-ui/index.html

Endpoints Principais:

GET /api/boletim/turmas

GET /api/boletim/disciplinas

GET /api/boletim/{turmaId}/{disciplinaId} (Carregamento da Tabela)

POST /api/boletim/salvar (Salvar Lançamentos)
