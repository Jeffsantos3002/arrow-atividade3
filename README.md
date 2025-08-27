# 🧪 Tutorial de Execução dos Testes de Mutação

Uma introdução ao teste de mutação, utilizando a ferramenta mutmut / mutatest no contexto da linguagem Python. A ideia central é avaliar a qualidade de uma suíte de testes, introduzindo pequenas alterações (mutantes) no código-fonte e verificando se os testes existentes conseguem “matar” esses mutantes (ou seja, detectar as alterações).

## 1.1. Entendendo o Projeto
O projeto de exemplo é uma implementação em Python do Arrow.py
https://github.com/arrow-py/arrow

## 1.2. Preparação do Ambiente
**Passo 1: Baixar o Projeto:** Comece baixando o projeto de exemplo do GitHub. Você pode clonar o repositório ou baixar o arquivo ZIP e extraí-lo.  
**Passo 2: Instalar Pré-requisitos:** Certifique-se de ter o Python 3.7+ instalado. O projeto utiliza um ambiente virtual, então você precisará do pacote python3-venv.  
**Passo 3: Criar e Ativar o Ambiente Virtual:** Navegue até o diretório do projeto no seu terminal. Crie um ambiente virtual com o comando:
```bash
python3 -m venv venv
```
Em seguida, ative-o:
```bash
source venv/bin/activate   # Linux / Mac
venv\Scripts\activate    # Windows
```
**Passo 4: Instalar as Bibliotecas Necessárias:** O projeto tem um arquivo requirements.txt. Instale as bibliotecas executando:
```bash
pip install -r requirements.txt
```
Isso instalará pytest, pytest-cov e outras.  
Alguns componentes tiveram problemas ao executar o mutmut, por isso, também utilizamos a biblioteca mutatest nesta análise.

### Instalação das Ferramentas
Para instalar o mutatest: 
```bash
pip install mutmut
```
Documentação do mutmut: https://mutmut.readthedocs.io/en/latest/

Para instalar o mutatest: 
```bash
pip install mutatest
```
Documentação do mutatest: https://mutatest.readthedocs.io/en/latest/

## 1.3. Executando os Testes Iniciais
**Passo 5: Executar Testes de Unidade:** Antes do teste de mutação, execute os testes existentes para garantir que estão passando.  
```bash
pytest testes
```
Ele irá rodar os testes de unidade e logo em seguida um relatório explicando sobre a cobertura de teste.  

**Passo 6: Verificar a Cobertura do Código:** Gere um relatório de cobertura para ver a porcentagem de código coberta pelos testes.  
```bash
pytest --cov=cal tests --cov-report html
```
Um relatório HTML será gerado, mostrando as partes do código que não estão sendo testadas.

## 1.4. Realizando o Teste de Mutação
**Passo 7: Rodar o mutatest:** Execute o teste de mutação com o comando  
```bash
mutatest --src arquivo.py
```
A ferramenta criará mutantes e executará os testes contra eles.  

**Rodar o mutmut:** Execute o teste de mutação com o comando  
```bash
mutmut run --paths-to-mutate arquivo.py
```

**Passo 8: Analisar os Resultados Iniciais:** O mutmut mostrará um resumo: quantos mutantes foram criados, quantos foram "mortos" (testes falharam) e quantos "sobreviveram" (testes passaram).

## 1.5. Melhorando a Suíte de Testes
**Passo 9: Ver Mutantes Sobreviventes:** Para analisar melhor os mutantes sobreviventes, você pode rodar com opção de relatório em JSON, por exemplo:  
```bash
mutatest --src arquivo.py --reporter json > resultado.json
```

Ver Mutantes Sobreviventes: Para ver a lista de mutantes que sobreviveram, use:  
```bash
mutmut results
```
Para inspecionar um mutante específico, use:  
```bash
mutmut show <id_do_mutante>
```
Exemplo:  
```bash
mutmut show 12
```

**Passo 10: Gerar Relatório Detalhado:** Outra forma é usar o relatório padrão em tabela:  
```bash
mutatest --src arquivo.py --reporter classic
```
Gerar Relatório HTML: Para uma visualização mais amigável, gere um relatório HTML com:  
```bash
mutmut html
```
Abra o arquivo `mutmut_html/cal.py.html` para ver o código original com as anotações dos mutantes que sobreviveram.

**Passo 11: Criar Novos Testes:** Analise os mutantes sobreviventes e crie novos casos de teste para "matá-los". Por exemplo, se um mutante alterou um `>` para um `>=`, adicione um teste que cubra essa condição de contorno.

**Passo 12: Rerun da Mutação:** Após adicionar os novos testes, execute novamente:  
- Com **mutatest**:  
```bash
mutatest --src arquivo.py
```
- Com **mutmut**:  
```bash
mutmut run --paths-to-mutate arquivo.py
```
Você deve ver uma melhoria na pontuação de mutação, com menos mutantes sobreviventes.

**Passo 13: Repetir o Processo:** Continue o ciclo de análise, criação de novos testes e execução até que a qualidade da sua suíte de testes seja satisfatória.
