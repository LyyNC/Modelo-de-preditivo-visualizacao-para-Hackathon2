# Hackton-Forecast-Big-data

Este projeto tem como objetivo prever a quantidade semanal de vendas por ponto de venda (PDV) e por produto (SKU) para as cinco semanas de janeiro de 2023. A previsão é baseada no histórico de vendas de todo o ano de 2022 e foi desenvolvida para apoiar decisões de abastecimento, reposição e planejamento operacional no varejo.

A solução foi construída com foco em escalabilidade, clareza e inteligência de negócio combinando técnicas de machine learning com conhecimento prático de varejo.


1. Integração dos dados
Começamos reunindo três fontes essenciais: catálogo de produtos, histórico de transações e catálogo de PDVs. Com isso, conseguimos cruzar o que foi vendido, onde e por quem, formando a base para entender o comportamento de consumo.

<img width="1142" height="356" alt="image" src="https://github.com/user-attachments/assets/c4fbd7ce-c485-4ee0-aa32-aabd1b95a500" />


2. Estruturação temporal
Transformamos datas em semanas e anos para alinhar os dados com o ritmo operacional do varejo. A previsão é semanal, então o modelo precisa enxergar os dados nesse mesmo compasso.

<img width="1152" height="148" alt="image" src="https://github.com/user-attachments/assets/eb86934d-8616-409f-a4b8-d5675c81758e" />


3. Foco no ano de 2022
Usamos apenas os dados de 2022 para garantir relevância e consistência. Isso evita que padrões antigos ou desatualizados interfiram na previsão de janeiro.

<img width="1151" height="122" alt="image" src="https://github.com/user-attachments/assets/2536da5b-9717-454c-a106-fab82ee567df" />


4. Agregação por semana, PDV e SKU
Agrupamos as vendas por semana, loja e produto, exatamente como o varejo pensa em reposição. Essa estrutura permite que o modelo aprenda padrões reais de consumo.


5. Criação da média móvel
Calculamos uma média móvel de 4 semanas para suavizar oscilações pontuais e destacar tendências. Essa feature ajuda o modelo a entender o ritmo de vendas sem depender de datas específicas.

<img width="1144" height="167" alt="image" src="https://github.com/user-attachments/assets/772d922e-c04b-4b9f-877d-9db6aa23cb1d" />


6. Enriquecimento com atributos
Incorporamos variáveis como categoria, subcategoria, marca e tipo de loja. Esses atributos são cruciais para capturar o contexto de cada venda, afinal, uma água mineral não se comporta como um shampoo, e uma loja de bairro não vende como um hipermercado.


7. Codificação categórica
Transformamos os atributos em números usando LabelEncoder, mantendo consistência entre treino e previsão futura. Isso permite que o modelo processe as informações corretamente.

<img width="1135" height="237" alt="image" src="https://github.com/user-attachments/assets/0c7d04ce-b535-48eb-b1b4-47139825cb3a" />


8. Tratamento da variável target
Aplicamos log na quantidade vendida para lidar com a distribuição assimétrica dos dados. Produtos com vendas muito altas ou muito baixas podem distorcer o aprendizado, o log ajuda a equilibrar isso.

<img width="1175" height="257" alt="image" src="https://github.com/user-attachments/assets/6e676bf0-ee85-4e2d-8b57-bb620f5b074d" />


9. Separação entre treino e teste
Dividimos os dados respeitando a ordem temporal, simulando uma previsão real. O modelo aprende com o passado e é testado com dados que ele nunca viu.

<img width="1157" height="312" alt="image" src="https://github.com/user-attachments/assets/9838e1d4-4511-40c0-b8d1-7833e59c78c5" />


10. Treinamento com LightGBM
Escolhemos o LightGBM por sua eficiência e capacidade de lidar com grandes volumes de dados tabulares. Ele entrega bons resultados com baixo custo computacional.

<img width="1045" height="738" alt="image" src="https://github.com/user-attachments/assets/1fc324fb-7f6b-4ada-a0e4-15418225726c" />

O MAPE ficou em 26.63%, o que é aceitável considerando que muitos produtos têm vendas baixas, e qualquer variação pequena gera um erro percentual alto.

<img width="245" height="96" alt="image" src="https://github.com/user-attachments/assets/e863d691-8459-436f-a828-ffc846609410" />


11. Avaliação do modelo
Calculamos métricas como MAE, RMSE, MAPE e acurácia dentro de ±10%. Também analisamos o desempenho por faixa de volume, porque no varejo, produtos de alto giro exigem mais precisão do que os esporádicos.

<img width="1047" height="394" alt="image" src="https://github.com/user-attachments/assets/6d7ede3f-b1f3-45e0-99f8-9f25396ac225" />

O modelo teve um desempenho bom para um primeiro ciclo. Ele erra, em média, 1 unidade por previsão (MAE = 1.08), e os erros maiores estão sob controle (RMSE = 3.58). A taxa de acerto dentro de uma margem de ±10% foi de 48.13%, o que significa que quase metade das previsões ficaram bem próximas do valor real.

<img width="253" height="639" alt="image" src="https://github.com/user-attachments/assets/3006187b-d608-4737-8ab9-14ec37e64400" />


12. Diagnóstico por faixa
Segmentamos os resultados em faixas como “baixo”, “médio” e “alto” volume. Isso revela onde o modelo está acertando bem e onde há espaço para ajustes.

<img width="231" height="138" alt="image" src="https://github.com/user-attachments/assets/e21cd67f-451e-4ba2-9609-949e753dd2bc" />

* Baixo (1 a 5 unidades): MAPE de 27.75%, produtos esporádicos são mais difíceis de prever.

* Médio (6 a 20 unidades): MAPE de 22.56%, o modelo começa a estabilizar.

* Alto (21 a 100 unidades): MAPE de 19.62%, previsões mais confiáveis.

* Muito alto (acima de 100 unidades): MAPE de 16.74%, melhor desempenho, modelo entende bem o padrão.


13. Previsão para janeiro/2023
Geramos uma grade com todos os pares PDV/SKU que venderam em 2022 e aplicamos o modelo para prever as cinco semanas de janeiro.

<img width="1044" height="700" alt="image" src="https://github.com/user-attachments/assets/f3c06de0-98b8-4abb-addd-779755f6c4ca" />

<img width="1031" height="360" alt="image" src="https://github.com/user-attachments/assets/d89ab43d-fb6b-4151-9086-95343e2adc5b" />





