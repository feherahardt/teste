## **1º Cenário:**

### **1\. Documentação e Materiais de Apoio:**

Primeiramente, leria a tarefa criada pelo Product Owner ou equipe de desenvolvimento para entender as funcionalidades a serem complementadas, o resultado esperado, as regras de negócio, os requisitos funcionais e não funcionais, além da jornada esperada. 

Em seguida, analisaria a documentação da integração para identificar os pontos de conexão com a implementação. Se a integração se conecta ao marketplace, por exemplo, é essencial compreender quais campos correspondem aos da plataforma e verificar se há regras de negócio envolvidas. Caso surjam dúvidas, buscaria esclarecimentos com a equipe técnica.

Por fim, utilizaria as ferramentas adotadas pela equipe técnica. Se o Jira for utilizado, por exemplo, documentaria os casos de teste com os passos executados, o comportamento observado, o comportamento esperado, o resultado (se passou ou não), além de observações e evidências (vídeos e prints). Para a leitura de manuais, geralmente faço isso de forma manual, recorrendo a ferramentas de IA para interpretar pontos específicos, se necessário.

### **2\. Abrangência dos Testes:**

Os casos de teste incluem:

* **Testes funcionais**: Validam se a funcionalidade atende aos requisitos esperados.  
* **Testes de integração**: Avaliam se a nova funcionalidade não impacta outras partes do sistema.  
* **Testes de entrada e saída de dados**: Verificam o formato dos dados, campos obrigatórios e limites de caracteres.  
* **Testes de UX/UI**: Avaliam a clareza e usabilidade para o usuário.  
* **Testes de responsividade e compatibilidade**: Conferem o comportamento em diferentes dispositivos e navegadores.  
* **Testes de desempenho**: Medem o tempo de resposta e o impacto no sistema.

### **3\. Execução dos Testes:**

A execução dos testes deve ser feita sempre em ambiente de desenvolvimento ou homologação, nunca em produção.

Os dados de teste podem ser criados ou baseados em exemplos reais de clientes (respeitando requisitos e regras), para simular o ambiente da forma mais fiel possível. Além disso, é essencial realizar os testes em um ambiente de homologação do marketplace em questão.

Nos testes manuais, a validação é feita como se um usuário estivesse utilizando o sistema. Quando há uma API disponível, os testes são realizados diretamente por meio dela. Atualmente, sigo esse processo e ainda não utilizei automação de testes, realizando testes manualmente ou via Swagger.

Os testes serão priorizados com base no impacto para o usuário final e na relevância para o negócio.


## **2º Cenário:**

Neste cenário, o processo seria semelhante ao do cenário 01, com a principal diferença nos testes realizados. Como se trata de estoque, seriam executados os seguintes testes:

* **Testes funcionais**: Cadastro de produto, baixa de produto, atualização do valor total do estoque, reabastecimento e remoção de produtos.  
* **Testes de integração**: Validação da integração com o Bling para garantir a atualização correta dos dados e a sincronização com vendas de produtos.  
* **Testes de entrada e saída de dados**: Verificação de dados obrigatórios e formato dos dados.  
* **Testes de UI/UX**: Avaliação da intuitividade e facilidade de uso, responsividade e compatibilidade com diferentes navegadores.  
* **Testes de desempenho**: Análise do tempo de resposta na busca do estoque, garantindo que os dados sejam exibidos rapidamente.


## **3º Cenário:**

Como não temos acesso ao painel do cliente, a melhor abordagem seria simular um estoque zerado em ambiente de teste. Para isso, selecionaria um produto com apenas uma unidade em estoque e removeria essa unidade até que o valor chegasse a zero. Dessa forma, seria possível validar se o problema ocorre de forma geral ou apenas para esse cliente específico.

Se o erro também acontecer no teste, há chances de que tenha havido alguma alteração na integração. Nesse caso, consultaria a documentação do Mercado Livre para buscar informações sobre possíveis mudanças. Caso não encontrasse uma explicação, o ideal seria abrir um chamado para a equipe técnica, relatando o problema e solicitando uma análise detalhada.

Se o problema **não** ocorrer no teste, o próximo passo seria verificar com a equipe técnica se há registros (tracks) das ações do cliente. Isso ajudaria a entender suas últimas interações e identificar eventuais falhas. Se os logs não estivessem disponíveis ou não fossem suficientes, seria necessário entrar em contato com o cliente para obter mais detalhes sobre a situação: como o problema aparece para ele, quais ações realizou e qualquer outra informação relevante.

O ideal é que, no atendimento inicial, todas essas informações sejam coletadas de forma completa, evitando contatos adicionais e tornando a investigação mais eficiente desde o primeiro atendimento.


## **4º Cenário:**

## **Campos alterados que precisam ser validados:**

* Nome completo  
* E-mail  
* Número de telefone  
* Data de nascimento  
* Endereço (Rua, Cidade, Estado e CEP)

### **Campos obrigatórios:**

* Nome completo  
* Tipo

### **Dúvidas sobre os dados:**

* Existe uma quantidade fixa de caracteres por campo?  
* Os campos "Número de telefone" e "Endereço" são obrigatórios ou opcionais?


### **Casos de Teste**

### **Caso 01 – Cadastro bem-sucedido**

**Ação:** Informar todos os dados corretamente e verificar se o cadastro é salvo.

**Passos:**

1. Clique em **Incluir Pessoa**.  
2. Na seção **Geral**:  
   * No campo **Tipo**, selecione "Física".  
   * No campo **Nome**, informe um nome completo.  
   * No campo **E-mail**, informe um e-mail válido (exemplo: email@gmail.com).  
3. Na seção **Endereço**:  
   * No campo **Rua**, informe um endereço completo.  
   * No campo **Cidade**, informe "Rio do Sul".  
   * No campo **Estado**, selecione "SC".  
   * No campo **CEP**, informe "89167-300".  
4. Na seção **Contato**:  
   * No campo **Número de telefone**, informe "(47) 9999-9999".  
5. Clique em **Gravar**.  
6. Consulte a pessoa recém-cadastrada e valide se os dados foram persistidos corretamente.

**Comportamento esperado:**

* O cadastro deve ser salvo com sucesso.  
* Uma mensagem de confirmação deve ser exibida ao usuário.  
* Os dados inseridos devem ser exibidos corretamente na consulta.

---

### **Caso 02 – Cadastro sem o campo obrigatório "Nome"**

**Ação:** Tentar cadastrar uma pessoa sem informar o nome.

**Passos:**

1. Repita os passos do Caso 01, mas deixe o campo **Nome** em branco.  
2. Clique em **Gravar**.

**Comportamento esperado:**

* O sistema **não deve permitir** o salvamento.  
* Uma mensagem deve ser exibida informando que o campo "Nome" é obrigatório.

---

### **Caso 03 – Cadastro sem o campo obrigatório "Tipo"**

**Ação:** Tentar cadastrar uma pessoa sem informar o tipo.

**Passos:**

1. Repita os passos do Caso 01, mas deixe o campo **Tipo** em branco.  
2. Clique em **Gravar**.

**Comportamento esperado:**

* O sistema **não deve permitir** o salvamento.  
* Uma mensagem deve ser exibida informando que o campo "Tipo" é obrigatório.

---

### **Caso 04 – Cadastro informando apenas o primeiro nome**

**Ação:** Informar apenas o primeiro nome no campo "Nome completo".

**Passos:**

1. Repita os passos do Caso 01, mas no campo **Nome**, informe apenas um primeiro nome (exemplo: "Carlos").  
2. Clique em **Gravar**.

**Comportamento esperado:**

* O sistema **não deve permitir** o salvamento.  
* Deve exibir uma mensagem informando que o nome completo (nome \+ sobrenome) deve ser preenchido.

---

### **Caso 05 – Cadastro com e-mail inválido**

**Ação:** Informar um e-mail inválido no cadastro.

**Passos:**

1. Repita os passos do Caso 01, mas no campo **E-mail**, informe um endereço inválido (exemplo: "email@").  
2. Clique em **Gravar**.

**Comportamento esperado:**

* O sistema **não deve permitir** o salvamento.  
* Deve exibir uma mensagem informando que o e-mail inserido não é válido.

---

### **Caso 06 – Cadastro com caracteres inválidos no telefone e CEP**

**Ação:** Informar caracteres inválidos nos campos "Telefone" e "CEP".

**Passos:**

1. Repita os passos do Caso 01, mas nos campos:  
   * **CEP**, informe "abacaxi".  
   * **Telefone**, informe "abacaxi".  
2. Clique em **Gravar**.

**Comportamento esperado:**

* O sistema **não deve permitir** a inserção de letras nesses campos.  
* Caso sejam digitados caracteres inválidos, eles devem ser ignorados automaticamente.  
* Se os campos forem gravados com valores inválidos, eles devem ser salvos como vazios.

---

### **Caso 07 – Alteração de pessoa existente**

**Ação:** Editar uma pessoa já cadastrada e validar se as alterações são salvas.

**Passos:**

1. Consulte uma pessoa já cadastrada.  
2. Altere os seguintes campos:  
   * **Seção Geral:** Tipo, Nome e E-mail.  
   * **Seção Endereço:** Rua, Cidade, Estado e CEP.  
   * **Seção Contato:** Número de telefone.  
3. Clique em **Gravar**.  
4. Consulte novamente a pessoa e valide se os dados foram atualizados corretamente.

**Comportamento esperado:**

* As alterações devem ser salvas corretamente.  
* Os novos dados devem ser exibidos ao consultar a pessoa.

---

### **Caso 08 – Consulta de pessoa cadastrada**

**Ação:** Consultar uma pessoa cadastrada e validar se os dados estão corretos.

**Passos:**

1. Consulte uma pessoa já cadastrada.  
2. Verifique se os dados exibidos correspondem aos dados inseridos no cadastro.

**Comportamento esperado:**

* A pessoa deve ser exibida na consulta.  
* Todos os dados devem estar corretos e iguais aos cadastrados.

---

