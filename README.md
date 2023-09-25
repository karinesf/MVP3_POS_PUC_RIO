# MVP3_POS_PUC_RIO

Trabalho realizado durante a terceira Sprint do Curso de Pós Graduação em Ciência de Dados e Analytics - PUC-RIO
1. Descrição:   
   Neste trabalho, será construido um pipeline de dados utilizando tecnologias na nuvem. O pipeline irá envolver a busca, coleta, modelagem, carga e análise dos dados. Assuntos vistos durante a sprint atual.
    1. Conjunto de dados utilizado: Loan-Approval-Prediction-Dataset
    2. Sobre o conjunto de dados:
       O conjunto de dados de aprovação de empréstimos é uma coleção de registros financeiros e informações associadas usadas para determinar a elegibilidade de indivíduos ou organizações para obter empréstimos de uma instituição de empréstimo. Ele inclui vários fatores, como pontuação cibil, renda, status de emprego, prazo do empréstimo, valor do empréstimo, valor dos ativos e status do empréstimo. Esse conjunto de dados é comumente usado em aprendizado de máquina e análise de dados para desenvolver modelos e algoritmos que preveem a probabilidade de aprovação de empréstimos com base nos recursos fornecidos.

3. Objetivo:
   Responder às seguintes perguntas:
    1. Ter Nível superior influencia na concessão do empréstimo?
    2. Como a quantidade de dependentes influencia no status do empréstimo?
    3. A Situação Laboral do Requerente influencia na concessão do empréstimo? 
    4. O valor do empréstimo afeta o status do empréstimo?
    5. A pontuação cibil é uma evidência forte para determinar a situação do empréstimo?
    6. O rendimento anual do Requerente influencia na concessão do empréstimo?
       
4. Detalhamento:
    1. Busca pelos dados:
       O dataset foi retirado da base de dados Kaggle no seguinte endereço:
       * [kaggle // Loan-Approval-Prediction-Dataset](https://www.kaggle.com/datasets/architsharma01/loan-approval-prediction-dataset?resource=download)
       ![fonte_de_dados](fonte_de_dados.png)
5. Coleta:
    1. Uma vez baixado a base de dados escolhida, foi feito o carregamento dela em um bucket no S3.
       -  Primeiro foi criado um bucket para armazenamento dos dados.
       ![cargaS3](cargaS3.png)
       - Depois foram criadas 2 pastas, onde em uma (aprovacao_emprestimo) será carregado os dados da base que irei utilizar e na outra (Dados_processados) que está vazia, irei carregar os dados depois de processados.
       ![cargaS3.1](cargaS3-1.png)
       - Aqui vemos a pasta com a base carregada no formato csv.
       ![cargaS3.2](cargaS3-2.png)
    2. Após essa etapa, foi criado um crawler no Data cataloge dentro do Glue.
       ![crawler](crawler.png)
       
    3. Nessa etapa, foi criado uma base de dados no data catalog dentro do AWS Glue.
       - O que ele faz é apontar para o arquivo que temos dentro do S3, para ser consultado por outras ferramentas nas fases seguintes.
       - Abaixo, observamos nossa base de dados e suas colunas.
         ![data-catalog](data-catalog.png)
       - Da mesma forma como criamos as pastas no S3, foram criadas 2 pastas no data catalog.
         ![data-catalog-1](data-catalog-1.png)
    4. Após essa etapa, vamos para o Glue studio para realizar a etapa de transformação dos dados.
       - Aqui observamos o job que foi criado. Não foi preciso fazer muitas alterações, apenas algumas transformações básicas.
        ![job](job.png)
       - Primeiro, foi selecionado a fonte de dados a ser utilizada, para melhor otimização do trabalho, puxei os dados da base criada no data catalog, que por sua vez, reflete os dados do S3.
        ![job-1](job-1.png)
        ![job-2](job-2.png)
       - Em seguida, foi utilizado a transformação de limpeza de linhas nulas na base.
        ![job-3](job-3.png)
       - Em seguida, foi utilizado a transformação de limpeza de dropar dados duplicados na base.
        ![job-4](job-4.png)
       - Por ultimo foi selecionado a base de dados de destino, onde serão armazenados os dados tratados.
        ![job-5](job-5.png)
       - Algumas configurações adicionais.
        ![job-6](job-6.png)
       - Aqui coloco os dados processados de volta no data catalog para análises porteriores.
        ![job-7](job-7.png)
        ![job-8](job-8.png)
       - Em seguida, o job é rodado e verificado se está tudo certo.
        ![job-9](job-9.png)
4. Agora irei utilizar o Athena, um serviço de consulta, onde irei fazer as querys para responder as perguntas iniciais.
   - Primeiro, foi configurado onde serão salvas as consultas realizadas.
    ![athena](athena.png)
   - Na primeira consulta puxei todos os campos da base de dados.
    ![athena-1](athena-1.png)
    ![athena-2](athena-2.png)
   - Na segunda consulta verifiquei a distribuição do campo que nos mostra se o emprestimo foi aprovado ou não. 
    ![athena-3](athena-3.png)
   - O resultado nos mostra que dos 4.269 dados disponíveis na base, temos 2.656 emprestimos que foram aprovados e 1.613 reprovados. Isso significa que temos mais dados que foram aprovados do que o comptrário.
    ![athena-4](athena-4.png)
   - Na terceira consulta Respondendo a pergunta 1: Ter nível superior influencia na concessão do empréstimo?
    ![athena-5](athena-5.png)
   - O resultado nos mostra que não temos uma diferença relevante entre quem é aprovado ou não em relação a ser graduado ou não.
    ![athena-6](athena-6.png)
   - Na quarta consulta Respondendo a pergunta 2: Como a quantidade de dependentes influencia no status do empréstimo?
    ![athena-7](athena-7.png)
   - O resultado nos mostra que não temos uma diferença relevante entre quem é aprovado ou não em relação a quantidade de dependentes. Temos casos em que não se tem dependentes e o emprestimo foi reprovado e muitos casos em que se tem 4 ou 5 dependentes e o emprestimo foi aprovado.
    ![athena-8](athena-8.png)
    ![athena-9](athena-9.png)
   - Na quinta consulta Respondendo a pergunta 3: A situação Laboral do Requerente influencia na concessão do empréstimo?
    ![athena-10](athena-10.png)
   - O resultado nos mostra mais uma vez que não temos uma diferença relevante entre quem é aprovado ou não em relação a situação laboral do contratante. 
    ![athena-11](athena-11.png)
    - Na sexta consulta Respondendo a pergunta 4: O rendimento anual do Requerente influencia na concessão do empréstimo?
    ![athena-12](athena-12.png)
   - Dessa vez, o resultado nos mostra que, quanto maior o rendimento anual do contratante, maior a quantidade e probabilidade dos emprestimos serem aprovados. Assim podemos inferir que sim, o rendimento anual do contratante tem influência na aprovação do emprestimo. 
    ![athena-13](athena-13.png)
    ![athena-14](athena-14.png)
   - Na sétima consulta Respondendo a pergunta 5: A pontuação cibil (score de crédito) é uma evidência forte para determinar a situação do empréstimo?
    ![athena-15](athena-15.png)
   - O resultado nos mostra que, sim, a pontuação de crédito influencia na aprovação do emprestimo. O primeiro resultado nos mostra o status do emprestimo e a pontuação da maior para a menor, e analisando a amostra na imagem, percebemos que pontuações altas tem uma grande quantidade de emprestimos aprovados.
    ![athena-16](athena-16.png)
   - Quando puxamos os dados com a pontuação da menor para a maior, percebemos que a quantidade de emprestimos reprovados aumenta.
    ![athena-17](athena-17.png)

5. Conclusões:
   - Por meio das análises realizadas nas etapas anteriores, foi possível explorar a base de dados e responder as perguntas elaboradas no inícil do projeto. Com os resultados obtidos, podemos inferir os campos de pontuação de crédito e o rendimento anual do contratante, estão fortemente associados a uma maior probabilidade de aprovação de empréstimos. Eles se destacam como os fatores de maior influência para as instituições financeiras na determinação da aprovação de empréstimos.
   - O trabalho percorre por questões de análises iniciais sobre a base, o que poderia ser aprimorado em uma versão porterior, explorando melhor a base, fazendo algumas transformações e utilizando ouros recursos para trazer mais visualizações e poder tirar mais insights dessa base de dados.
