# MVP3_POS_PUC_RIO

Trabalho realizado durante a terceira Sprint do Curso de Pós Graduação em Ciência de Dados e Analytics -PUC-RIO
1. Descrição:   
   Neste trabalho, será construído um pipeline de dados utilizando tecnologias na nuvem. O pipeline irá envolver a busca, coleta, modelagem, carga e análise dos dados. Assuntos vistos durante a sprint atual.
    1. Conjunto de dados utilizado: Loan-Approval-Prediction-Dataset
    2. Sobre o conjunto de dados:
       O conjunto de dados de aprovação de empréstimos é uma coleção de registros financeiros e informações associadas usadas para determinar a elegibilidade de indivíduos ou organizações para obter empréstimos de uma instituição de empréstimo. Ele inclui vários fatores, como pontuação cibil, renda, status de emprego, prazo do empréstimo, valor do empréstimo, valor dos ativos e status do empréstimo. Esse conjunto de dados é comumente usado em aprendizado de máquina e análise de dados para desenvolver modelos e algoritmos que preveem a probabilidade de aprovação de empréstimos com base nos recursos fornecidos.

3. Objetivo:
   Responder às seguintes perguntas:
    1. Ter nível superior tem influência na concessão do empréstimo?
    2. Como a quantidade de dependentes tem influência no status do empréstimo?
    3. A Situação Laboral do Requerente tem influência na concessão do empréstimo? 
    4. O valor do empréstimo afeta o status do empréstimo?
    5. A pontuação cibil é uma evidência forte para determinar a situação do empréstimo?
    6. O rendimento anual do Requerente tem influência na concessão do empréstimo?
       
4. Detalhamento:
    1. Busca pelos dados:
       O dataset foi retirado da base de dados Kaggle no seguinte endereço:
       * [kaggle // Loan-Approval-Prediction-Dataset](https://www.kaggle.com/datasets/architsharma01/loan-approval-prediction-dataset?resource=download)
         
       ![fonte_de_dados](print/fonte_de_dados.png)
       
5. Coleta:
    1. Uma vez baixada a base de dados escolhida, foi feito o carregamento dela em um bucket no S3.
       -  Primeiro foi criado um bucket para armazenamento dos dados.
       
       ![cargaS3](print/cargaS3.png)
       
       - Depois foram criadas 2 pastas, onde em uma (aprovacao_emprestimo) foi carregado os dados da base que irei utilizar e na outra (Dados_processados) que está vazia, irei carregar os dados depois de processados.
         
       ![CargaS3-1](print/CargaS3-1.png)
       
       - Aqui vemos a pasta com a base carregada no formato csv.
         
       ![cargaS3-2](print/cargaS3-2.png)
       
    3. Após essa etapa, foi criado um crawler no Data cataloge dentro do Glue.
       ![crawler](print/crawler.png)
       
    4. Nessa etapa, foi criado uma base de dados no data catalog dentro do AWS Glue.
       - O que ele faz é apontar para o arquivo que temos dentro do S3, para ser consultado por outras ferramentas nas fases seguintes.
       - Abaixo, observamos nossa base de dados e suas colunas.
         
         ![data-catalog](print/data-catalog.png)
         
       - Da mesma forma como criamos as pastas no S3, foram criadas 2 pastas no data catalog.
         
         ![data-catalog-1](print/data-catalog-1.png)
         
    5. Após essa etapa, vamos para o Glue studio para realizar a etapa de transformação dos dados.
       - Aqui observamos o job que foi criado. Não foi preciso fazer muitas alterações, apenas algumas transformações básicas.
         
        ![job](print/job.png)
       
       - Primeiro, foi selecionada a fonte de dados a ser utilizada, para melhor otimização do trabalho, puxei os dados da base criada no data catalog, que por sua vez, reflete os dados do S3.
         
        ![job-1](print/job-1.png)
        ![job-2](print/job-2.png)
       
       - Em seguida, foi feito a transformação de limpeza de linhas nulas na base.
         
        ![job-3](print/job-3.png)
       
       - Em seguida, foi utilizado a transformação de limpeza de dropar dados duplicados na base.
         
        ![job-4](print/job-4.png)
   
       - Por último, foi selecionada a base de dados de destino, onde serão armazenados os dados tratados.
         
        ![job-5](print/job-5.png)
       
       - Algumas configurações adicionais.
         
        ![job-6](print/job-6.png)
       
       - Aqui, coloco os dados processados de volta no data catalog para análises posteriores.
         
        ![job-7](print/job-7.png)
        ![job-8](print/job-8.png)
       
       - Em seguida, o job é rodado e verificado se está tudo certo.
         
        ![job-9](print/job-9.png)
       
4. Agora irei utilizar o Athena, um serviço de consulta, onde irei fazer as querys para responder às perguntas iniciais.
   - Primeiro, foi configurado onde serão salvas as consultas realizadas.
     
    ![athena](print/athena.png)
   
   - Na primeira consulta, puxei todos os campos da base de dados.
     
    ![athena-1](print/athena-1.png)
    ![athena-2](print/athena-2.png)
   
   - Na segunda consulta, verifiquei a distribuição do campo que nos mostra se o empréstimo foi aprovado ou não.
     
    ![athena-3](print/athena-3.png)
   
   - O resultado nos mostra que dos 4.269 dados disponíveis na base, temos 2.656 empréstimos que foram aprovados e 1.613 que foram reprovados. Isso significa que temos mais dados que foram aprovados do que o contrário.
     
    ![athena-4](print/athena-4.png)
   
   - Na terceira consulta, respondendo a pergunta 1: Ter nível superior tem influência na concessão do empréstimo?
     
    ![athena-5](print/athena-5.png)
   
   - O resultado nos mostra que não temos uma diferença relevante entre quem é aprovado ou não em relação a ser graduado ou não.
     
    ![athena-6](print/athena-6.png)
   
   - Na quarta consulta, respondendo a pergunta 2: Como a quantidade de dependentes tem influência no status do empréstimo?
     
    ![athena-7](print/athena-7.png)
   
   - O resultado nos mostra que não temos uma diferença relevante entre quem é aprovado ou não em relação a quantidade de dependentes. Temos casos em que não se tem dependentes e o empréstimo foi reprovado e muitos casos em que se tem 4 ou 5 dependentes e o empréstimo foi aprovado.
     
    ![athena-8](print/athena-8.png)
    ![athena-9](print/athena-9.png)
   
   - Na quinta consulta, respondendo a pergunta 3: A situação Laboral do Requerente tem influência na concessão do empréstimo?
     
    ![athena-10](print/athena-10.png)
   
   - O resultado nos mostra mais uma vez que não temos uma diferença relevante entre quem é aprovado ou não em relação à situação laboral do contratante.
     
    ![athena-11](print/athena-11.png)
   
    - Na sexta consulta, respondendo a pergunta 4: O rendimento anual do Requerente tem influência na concessão do empréstimo?
      
    ![athena-12](print/athena-12.png)
   
   - Dessa vez, o resultado nos mostra que, quanto maior o rendimento anual do contratante, maior a quantidade e probabilidade dos empréstimos serem aprovados. Assim podemos inferir que sim, o rendimento anual do contratante tem influência na aprovação do emprestimo.
     
    ![athena-13](print/athena-13.png)
    ![athena-14](print/athena-14.png)
   
   - Na sétima consulta, respondendo a pergunta 5: A pontuação cibil (score de crédito) é uma evidência forte para determinar a situação do empréstimo?
     
    ![athena-15](print/athena-15.png)
   
   - O resultado nos mostra que, sim, a pontuação de crédito tem influência na aprovação do empréstimo. O primeiro resultado nos mostra o status do empréstimo e a pontuação da maior para a menor, e analisando a amostra na imagem, percebemos que pontuações altas tem uma grande quantidade de empréstimos que foram aprovados.
     
    ![athena-16](print/athena-16.png)
   
   - Quando puxamos os dados com a pontuação da menor para a maior, percebemos que a quantidade de empréstimos reprovados aumenta.
     
    ![athena-17](print/athena-17.png)

6. Conclusões:
   - Por meio das análises realizadas nas etapas anteriores, foi possível explorar a base de dados e responder às perguntas elaboradas no início do projeto. Com os resultados obtidos, podemos inferir que os campos de pontuação de crédito e o rendimento anual do contratante, estão fortemente associados a uma maior probabilidade de aprovação de empréstimos. Eles se destacam como os fatores de maior influência para as instituições financeiras na determinação da aprovação de empréstimos.
   - O trabalho percorre por questões de análises iniciais sobre a base, o que poderia ser aprimorado em uma versão posterior, explorando melhor a base, fazendo algumas transformações e utilizando outros recursos para trazer mais visualizações e poder tirar mais insights dessa base de dados.
