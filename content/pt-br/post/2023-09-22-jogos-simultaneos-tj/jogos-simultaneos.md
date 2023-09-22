---
title: 'Teoria dos Jogos no R: Jogos Simultâneos'
author: André Melo
date: '2023-09-16'
math: true
thumbnail:
    src: 'images/jogos-simultaneos.jpg'
    alt: 'Teoria dos Jogos no R: Jogos Simultâneos'
    object_position: '50% 100%'
    height: 300px
showToc: true
slug: jogos-simultaneos-tj
categories: 
 - Tutorial
tags: 
 - teoriadosjogos
 - r
 - economia
---
<script src="/rproject/rmarkdown-libs/kePrint/kePrint.js"></script>
<link href="/rproject/rmarkdown-libs/lightable/lightable.css" rel="stylesheet" />

Este tutorial tem como objetivo ensinar como aplicar a teoria dos jogos no ambiente R, utilizando o pacote Rgamer.

<!--more-->

<div style="text-align:center;">
  <img src="https://raw.githubusercontent.com/yukiyanai/rgamer/master/logo.png" alt="Rgamer" width="200">
</div>

<style type="text/css">

/* Estilize a tabela */
table.lightable-classic {
    border-collapse: collapse;
    width: 100%;
    font-family: Arial;
}

/* Estilize o cabeçalho da tabela */
table.lightable-classic th {
    text-align: center;
    background-color: #f2f2f2; /* Cor de fundo do cabeçalho */
    font-weight: bold;
    padding: 8px;
    border: 1px solid #ddd;
}

/* Estilize as células da tabela */
table.lightable-classic td {
    text-align: center;
    padding: 8px;
    border: 1px solid #ddd;
}

/* Estilize as células de cabeçalho da primeira linha */
table.lightable-classic thead th {
    border-top: none;
}

/* Estilize as células de dados */
table.lightable-classic tbody td {
    background-color: #fff; /* Cor de fundo das células de dados */
}

/* Estilize células de dados alternadas para um efeito listrado */
table.lightable-classic tbody tr:nth-child(even) td {
    background-color: #f2f2f2;
}

/* Estilize a primeira coluna da tabela */
table.lightable-classic tbody td:first-child {
    font-weight: bold;
    text-align: left;
}

</style>

# 1. Introdução

A teoria dos jogos é um ramo da matemática que explora as interações estratégicas em que as decisões de um indivíduo são influenciadas pelo conflito e cooperação de outros participantes. Em relação ao campo das ciências econômicas, essa teoria desempenha um papel fundamental ao analisar como as pessoas realizam escolhas estratégicas diante de objetivos concorrentes em um contexto de escassez.

Uma aplicação interessante da teoria dos jogos na área da economia é a sua combinação com simulações de aprendizado. Por exemplo, em um cenário em que diferentes empresas competem para estabelecer preços em um mercado, é possível utilizar a teoria dos jogos em conjunto com modelos de aprendizado. Nesses modelos, as empresas podem aprender ao longo do tempo a tomar decisões estratégicas mais eficientes e adaptar suas estratégias com base nas escolhas e resultados passados. Dessa forma, as empresas podem ajustar gradualmente suas ações para obter melhores resultados e alcançar equilíbrios de longo prazo em um ambiente competitivo.

Este tutorial tem como objetivo ensinar como aplicar a teoria dos jogos no ambiente R, utilizando o pacote <a href="https://github.com/yukiyanai/rgamer" target="_blank" style="color:#016dea; text-decoration: none;" onmouseover="this.style.color='#014ba0';" onmouseout="this.style.color='#016dea';">Rgamer</a>, desenvolvido por Yusei Yanai, Ph.D. em Ciência Política, e Yoshio Kamijo, Especialista em Economia Experimental, Teoria dos Jogos, Teoria Comportamental dos Jogos e Design do Futuro. Desse modo, através da integração de funções de modelos de aprendizado, exploraremos a teoria dos jogos e sua aplicação prática. O pacote oferece representações visuais de jogos para dois ou mais jogadores e possibilita encontrar equilíbrios de Nash, proporcionando uma compreensão mais profunda dos conceitos fundamentais no contexto das Ciências Econômicas. Com o <a href="https://github.com/yukiyanai/rgamer" target="_blank" style="color:#016dea; text-decoration: none;" onmouseover="this.style.color='#014ba0';" onmouseout="this.style.color='#016dea';">Rgamer</a>, tem-se disponível uma ferramenta poderosa para a análise e simulação de interações estratégicas, tornando o estudo do assunto ainda mais acessível e abrangente.

## 1.1 Pacotes

Para realização dos passos seguintes, será necessário a instalação e ativação do pacote:


```r
# install.packages("devtools")
# devtools::install_github("yukiyanai/rgamer")
library(rgamer)
library(utf8)
```

# 2. Jogos Simultâneos

Os jogos simultâneos representam uma categoria fundamental na teoria dos jogos, na qual todos os jogadores tomam suas decisões ao mesmo tempo, sem qualquer conhecimento prévio das escolhas dos outros jogadores. No ambiente de programação R, o pacote Rgamer oferece uma ampla gama de recursos que facilitam a modelagem de jogos simultâneos, o cálculo de equilíbrios de Nash, a representação gráfica e a análise de estratégias dominantes.

## 2.1 Estratégias Puras

O dilema dos prisioneiros é um clássico exemplo de jogo de estratégia pura em teoria dos jogos que envolve dois jogadores que enfrentam uma escolha difícil entre cooperar ou trair o outro jogador.  Através da função <span class="highlighted-text">`normal_form()`</span> teremos dois exemplos desse tipo forma normal de jogo em dois métodos.

<div id="botoes">
  <button id="botao-metodo1" class="botao-interativo" onclick="showConteudo('metodo1')">Método 1</button>
  <button id="botao-metodo2" class="botao-interativo" onclick="showConteudo('metodo2')">Método 2</button>
</div>

<script>
window.onload = function() {
    showConteudoExemplo('exemplo1'); // Exibir conteúdo do exemplo1 por padrão
    showConteudo('metodo1'); // Exibir conteúdo do método1 por padrão
  };
</script>

<script>
function showConteudo(conteudoId) {
  var conteudos = document.getElementsByClassName('conteudo');
  for (var i = 0; i < conteudos.length; i++) {
    conteudos[i].style.opacity = 0; // Definir a opacidade do conteúdo como 0 (invisível)
    conteudos[i].style.display = 'none'; // Esconder o conteúdo (display: none)
  }
  
  // Exibir o conteúdo desejado com animação suave
  var conteudoDesejado = document.getElementById(conteudoId);
  conteudoDesejado.style.display = 'block'; // Exibir o conteúdo (display: block)
  setTimeout(function() {
    conteudoDesejado.style.opacity = 1; // Definir a opacidade do conteúdo como 1 (visível)
  }, 50); // Aguardar 50 milissegundos para aplicar a opacidade (ajuste conforme desejado)

  // Remover a classe 'selecionado' de todos os botões
  var botoes = document.getElementsByClassName('botao-interativo');
  for (var i = 0; i < botoes.length; i++) {
    botoes[i].classList.remove('selecionado');
  }

  // Adicionar a classe 'selecionado' apenas ao botão clicado
  var botaoSelecionado = document.getElementById('botao-' + conteudoId);
  botaoSelecionado.classList.add('selecionado');
}
</script>

<style type="text/css">
/* JOGOS SIMULTÂNEOS-ESTRATÉGIAS PURAS -- COMEÇO */
   #botoes {
    display: flex;
    justify-content: center;
  }
  .botao-interativo {
    background-color: transparent; /* Fundo transparente para todos os botões */
    border-color: transparent; /* Borda transparente para todos os botões */
    margin-left: 10px;
    color: #016dea; /* Cor do texto para todos os botões */
    border-radius: 0.5rem;
    width: 120px; /* Largura do botão */
    height: 40px; /* Altura do botão */
    font-size: 15px; /* Tamanho da fonte do texto do botão */
    transition: background-color 0.3s; /* Adicionando uma transição suave de 0.3 segundos */
  width: auto; /* Largura automática para ajustar ao tamanho do texto */
    white-space: nowrap; /* Evita que o texto quebre em várias linhas */
  }

  .botao-interativo:hover {
    background-color: #E5E5E5; /* Cor cinza claro quando o mouse passar por cima */
    color: #002e63;
  }
  
  .botao-interativo.selecionado {
    background-color: #0766a6;
    color: white; /* Cor do texto do botão selecionado */
  }
  
  .conteudo {
    opacity: 0; /* Começa com opacidade 0 (invisível) */
    transition: opacity 0.5s;
  }
  
/* JOGOS SIMULTÂNEOS-ESTRATÉGIAS PURAS -- FIM */

/* JOGOS SIMULTÂNEOS-MISTAS -- COMEÇO */
  #botoesExemplo {
    display: flex;
    justify-content: center;
  }
  .botao-interativo-exemplo {
    background-color: transparent;
    border-color: transparent;
    margin-left: 10px; /* Adicionando margem para separar os botões */
    padding: 5px 10px; /* Adicionando espaçamento interno para melhor aparência */
    color: #016dea;
    border-radius: 0.5rem;
    font-size: 15px;
    transition: background-color 0.3s;
    width: auto; /* Largura automática para ajustar ao tamanho do texto */
    white-space: nowrap; /* Evita que o texto quebre em várias linhas */
  }

  .botao-interativo-exemplo:hover {
    background-color: #E5E5E5;
    color: #002e63;
  }

  .botao-interativo-exemplo.selecionadoExemplo {
    background-color: #0766a6;
    color: white;
  }

  .conteudoExemplo {
    opacity: 0;
    transition: opacity 0.5s;
  }

/* JOGOS SIMULTÂNEOS-MISTAS -- FIM */
</style>

<div id="metodo1" class="conteudo">

Neste primeiro método, é necessário definir os pagamentos para cada célula da matriz do jogo utilizando o argumento <span class="highlighted-text">`cells`</span>, que é configurado como uma lista, no caso <span class="highlighted-text">`list()`</span>. Também é necessário especificar os jogadores, que, neste exemplo, são <span class="highlighted-text">`"Bonnie"`</span> e <span class="highlighted-text">`"Clyde"`</span>, utilizando o argumento <span class="highlighted-text">`players`</span>, em que as estratégias, <span class="highlighted-text">`"Confessar"`</span> e <span class="highlighted-text">`"Não Confessar"`</span>, respectivamente, são definidas nos argumentos <span class="highlighted-text">`s1`</span> e <span class="highlighted-text">`s2`</span> para eles.

O argumento lógico <span class="highlighted-text">`byrow`</span> pode ser configurado como <span class="highlighted-text">`TRUE`</span> ou <span class="highlighted-text">`FALSE`</span>. Quando definido como <span class="highlighted-text">`TRUE`</span>, os valores na lista dentro das células são especificados por linha, seguindo a ordem das estratégias "Confessar" e "Não Confessar", respectivamente, para cada jogador. Se configurado como <span class="highlighted-text">`FALSE`</span>, os valores serão organizados por coluna.


```r
jogo1 <- normal_form(
          players = c("Bonnie", "Clyde"),
          s1 = c("Confessar", "Não Confessar"), 
          s2 = c("Confessar", "Não Confessar"), 
          cells = list(c(-8, -8), 
                       c(0, -15),
                       c(-15, 0), 
                       c(-1, -1)),
          byrow = TRUE)
```

Para encontrar a solução do primeiro método é necessário a função <span class="highlighted-text">`solve_nfg()`</span> e o argumento <span class="highlighted-text">`show_table = TRUE`</span> para obter a tabela e o equilíbrio de Nash. 


```r
s_jogo1 <- solve_nfg(jogo1, show_table = TRUE)
```

```
Pure-strategy NE: [Confessar, Confessar]
```

<table class=" lightable-classic table" style="font-family: Arial; margin-left: auto; margin-right: auto; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr>
<th style="empty-cells: hide;" colspan="2"></th>
<th style="padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; font-weight: bold; " colspan="2"><div style="border-bottom: 1px solid #111111; margin-bottom: -1px; ">Clyde</div></th>
</tr>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> strategy </th>
   <th style="text-align:center;"> Confessar </th>
   <th style="text-align:center;"> Não Confessar </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-weight: bold;"> Bonnie </td>
   <td style="text-align:center;"> Confessar </td>
   <td style="text-align:center;"> -8^, -8^ </td>
   <td style="text-align:center;"> 0^, -15 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Não Confessar </td>
   <td style="text-align:center;"> -15, 0^ </td>
   <td style="text-align:center;"> -1, -1 </td>
  </tr>
</tbody>
</table>

Considerando o fator "traição", escolher confessar é sempre a melhor opção para Bonnie, independentemente da escolha de Clyde. A estratégia de não confessar pode levar a resultados piores se o outro jogador escolher confessar. Portanto, confessar é a estratégia dominante para ambos, incentivando a cooperação mútua.

</div>

<div id="metodo2" class="conteudo" style="display: none;">

No segundo método, por padrão, temos os <span class="highlighted-text">`payoffs1`</span> e <span class="highlighted-text">`payoffs2`</span>, onde especificamos os ganhos de cada jogador. Neste outro exemplo dois postos de gasolina, <span class="highlighted-text">`"OilFlex"`</span> e <span class="highlighted-text">`"EconoGas"`</span>, competem para ver quem obtém o maior lucro e conquista a maior fatia de mercado na venda de combustíveis. Nesse cenário, as estratégias definidas para cada posto serão <span class="highlighted-text">`"Manter Preço"`</span> e <span class="highlighted-text">`"Reduzir Preço"`</span>.


```r
jogo2 <- normal_form(
          players = c("OilFlex", "EconoGas"),
          s1 = c("Manter Preço", "Reduzir Preço"), 
          s2 = c("Manter Preço", "Reduzir Preço"), 
          payoffs1 = c(50, 60, 30, 40), 
          payoffs2 = c(50, 30, 60, 40))
```

Para obter a solução utilizando o segundo método, assim como no primeiro, também é possível definir o argumento <span class="highlighted-text">`show_table`</span> como <span class="highlighted-text">`FALSE`</span>.


```r
s_jogo2 <- solve_nfg(jogo2, show_table = FALSE)
```

```
Pure-strategy NE: [Reduzir Preço, Reduzir Preço]
```

Sendo necessário o comando <span class="highlighted-text">`$table`</span> para mostrar a tabela, juntamente com o equilíbrio de Nash.


```r
s_jogo2$table
```

<table class=" lightable-classic table" style="font-family: Arial; margin-left: auto; margin-right: auto; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr>
<th style="empty-cells: hide;" colspan="2"></th>
<th style="padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; font-weight: bold; " colspan="2"><div style="border-bottom: 1px solid #111111; margin-bottom: -1px; ">EconoGas</div></th>
</tr>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> strategy </th>
   <th style="text-align:center;"> Manter Preço </th>
   <th style="text-align:center;"> Reduzir Preço </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-weight: bold;"> OilFlex </td>
   <td style="text-align:center;"> Manter Preço </td>
   <td style="text-align:center;"> 50, 50 </td>
   <td style="text-align:center;"> 30, 60^ </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Reduzir Preço </td>
   <td style="text-align:center;"> 60^, 30 </td>
   <td style="text-align:center;"> 40^, 40^ </td>
  </tr>
</tbody>
</table>

Se os dois concordarem em manter o preço original, cada um ganha 50 mil por mês. Se um deles reduzir o preço, ele recebe 60 mil, enquanto o outro ganha apenas 30 mil. Caso ambos decidam reduzir o preço, cada um ganha 40 mil. O ponto de equilíbrio é quando ambos reduzem o preço, pois essa é a estratégia dominante e resulta em um equilíbrio de Nash sem cooperação.

</div>

## 2.2 Estratégias Dominantes

As estratégias estritamente dominantes referem-se a uma situação em que um jogador tem uma estratégia que é sempre melhor do que qualquer outra estratégia que ele possa escolher, independentemente das escolhas dos outros jogadores.

No exemplo a seguir, os jogadores <span class="highlighted-text">`"Pedro"`</span> e <span class="highlighted-text">`"Ana"`</span> são especificados no argumento <span class="highlighted-text">`players`</span>. Neste jogo, ao contrário dos dois exemplos anteriores, cada jogador possui 3 estratégias disponíveis: <span class="highlighted-text">`"Bar"`</span>, <span class="highlighted-text">`"Museu"`</span> e <span class="highlighted-text">`"Café"`</span>. Dessa forma, será necessário aumentar o número de elementos nas matrizes <span class="highlighted-text">`payoffs1`</span> e <span class="highlighted-text">`payoffs2`</span>, agora contendo um total de 9 elementos.


```r
jogo3 <- normal_form(
          players = c("Pedro", "Ana"),
          s1 = c("Bar", "Museu", "Café"), 
          s2 = c("Bar", "Museu", "Café"), 
          payoffs1 = c(6, 2, 1, 
                       6, 5, 1, 
                       4, 2, 3), 
          payoffs2 = c(4, 1, 1, 
                       3, 5, 3, 
                       2, 2, 6))
```

O argumento lógico <span class="highlighted-text">`mark_br`</span> desempenha a função de destacar as melhores respostas (*best responses*) na tabela de solução. Quando é definido como <span class="highlighted-text">`FALSE`</span>, as melhores respostas não são destacadas na tabela. O valor padrão para <span class="highlighted-text">`mark_br`</span> é <span class="highlighted-text">`TRUE`</span>, o que significa que, caso esse argumento na função não seja incluído na função, a tabela exibirá automaticamente o equilíbrio de Nash.

Nesse caso, o equilíbrio de Nash em conjunto com a mensagem não serão exibidas, já que ao configurar <span class="highlighted-text">`quietly`</span> como <span class="highlighted-text">`TRUE`</span>, está sendo implicitamente ocultado a mensagem de melhores respostas. Essa configuração flexível permite controlar tanto a exibição da mensagem quanto o destaque do Equilíbrio de Nash, de acordo com as preferências.


```r
s_jogo3 <- solve_nfg(jogo3, 
                     show_table = TRUE, 
                     mark_br = FALSE, 
                     quietly = TRUE)
```

<table class=" lightable-classic table" style="font-family: Arial; margin-left: auto; margin-right: auto; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr>
<th style="empty-cells: hide;" colspan="2"></th>
<th style="padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; font-weight: bold; " colspan="3"><div style="border-bottom: 1px solid #111111; margin-bottom: -1px; ">Ana</div></th>
</tr>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> strategy </th>
   <th style="text-align:center;"> Bar </th>
   <th style="text-align:center;"> Museu </th>
   <th style="text-align:center;"> Café </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-weight: bold;"> Pedro </td>
   <td style="text-align:center;"> Bar </td>
   <td style="text-align:center;"> 6, 4 </td>
   <td style="text-align:center;"> 6, 3 </td>
   <td style="text-align:center;"> 4, 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Museu </td>
   <td style="text-align:center;"> 2, 1 </td>
   <td style="text-align:center;"> 5, 5 </td>
   <td style="text-align:center;"> 2, 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Café </td>
   <td style="text-align:center;"> 1, 1 </td>
   <td style="text-align:center;"> 1, 3 </td>
   <td style="text-align:center;"> 3, 6 </td>
  </tr>
</tbody>
</table>

Na função <span class="highlighted-text">`dom()`</span> com <span class="highlighted-text">`type = "dominant"`</span>, são identificadas as estratégias estritamente dominantes para cada jogador no jogo. Por meio dessa configuração é possível identificar as estratégias que são sempre a melhor escolha, independentemente das ações tomadas pelos outros jogadores. 

Diferentemente do parâmetro <span class="highlighted-text">`quietly`</span>  encontrado em funções como <span class="highlighted-text">`solve_nfg()`</span> e outras do pacote Rgamer, é essencial demonstrar que ao configurar esse parâmetro como <span class="highlighted-text">`FALSE`</span> na função <span class="highlighted-text">`dom()`</span>, ela passa a exibir uma mensagem que identifica quais estratégias são estritamente dominantes.


```r
dominant_j3 <- dom(jogo3, 
                   type = "dominant", 
                   quietly = FALSE)
```

```
Pedro's dominant strategy: Bar
```

```
Pedro's weakly dominant strategy: Bar
```

```
Ana's dominant strategy: NA
```

```
Ana's weakly dominant strategy: NA
```

No primeiro conjunto de resultados, ao analisar as estratégias estritamente dominantes, identificamos que a estratégia "Bar" é a melhor resposta para Pedro. Nesse contexto, essa estratégia é a escolha que Pedro deve realizar independentemente das escolhas de Ana.

Contudo, é importante ressaltar que a existência de estratégias estritamente dominantes em jogos não é uma característica universal, variando conforme as regras e as interações específicas dentro de cada jogo. Desse modo, quando tais estratégias estão presentes, elas desempenham um papel crucial na análise e na formulação de decisões estratégicas.

## 2.3 Estratégias Dominadas

A dominância fraca é um dos critérios usados para identificar estratégias não razoáveis ou não ótimas em um jogo. Ela indica a existência de outra estratégia que é, no mínimo, tão boa quanto a estratégia dominada fracamente, mas que pode oferecer um melhor resultado em algumas situações específicas.

No mesmo exemplo, faremos uma alteração no parâmetro <span class="highlighted-text">`type`</span>, em que anteriormente estava definido como <span class="highlighted-text">`"dominant"`</span>, mas agora será configurado como <span class="highlighted-text">`"dominated"`</span>. O objetivo dessa modificação é identificar estratégias que são dominadas e fracamente dominadas tanto para Pedro quanto para Ana.


```r
dominated_j3 <- dom(jogo3, 
                    type = "dominated", 
                    quietly = FALSE)
```

```
Pedro's dominated strategy: Museu, Café
```

```
Pedro's weakly dominated strategy: Museu, Café
```

```
Ana's dominated strategy: NA
```

```
Ana's weakly dominated strategy: NA
```

Através da análise do segundo conjunto, podemos evidenciar que para o jogador Pedro, as estratégias "Museu" e "Café" são dominadas por outra estratégia, pois sempre há uma escolha alternativa, no caso "Bar", que leva a um resultado melhor, independentemente da escolha do outro jogador.

Na perspectiva de Ana, em ambos os conjuntos de resultados, não foram encontradas estratégias dominadas nem dominantes, o que indica que suas escolhas são consideradas melhores respostas dadas as escolhas de Pedro. 


```r
br_j3 <- solve_nfg(jogo3)
```

```
Pure-strategy NE: [Bar, Bar]
```

<table class=" lightable-classic table" style="font-family: Arial; margin-left: auto; margin-right: auto; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr>
<th style="empty-cells: hide;" colspan="2"></th>
<th style="padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; font-weight: bold; " colspan="3"><div style="border-bottom: 1px solid #111111; margin-bottom: -1px; ">Ana</div></th>
</tr>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> strategy </th>
   <th style="text-align:center;"> Bar </th>
   <th style="text-align:center;"> Museu </th>
   <th style="text-align:center;"> Café </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-weight: bold;"> Pedro </td>
   <td style="text-align:center;"> Bar </td>
   <td style="text-align:center;"> 6^, 4^ </td>
   <td style="text-align:center;"> 6^, 3 </td>
   <td style="text-align:center;"> 4^, 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Museu </td>
   <td style="text-align:center;"> 2, 1 </td>
   <td style="text-align:center;"> 5, 5^ </td>
   <td style="text-align:center;"> 2, 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;font-weight: bold;">  </td>
   <td style="text-align:center;"> Café </td>
   <td style="text-align:center;"> 1, 1 </td>
   <td style="text-align:center;"> 1, 3 </td>
   <td style="text-align:center;"> 3, 6^ </td>
  </tr>
</tbody>
</table>

Agora, ao verificar as melhores estratégias para o jogador 1 e 2, a mensagem e o destaque do Equilíbrio de Nash podem ser exibidos. Dentro da estrutura da função <span class="highlighted-text">`solve_nfg()`</span>, ao remover o parâmetro <span class="highlighted-text">`show_table`</span>, a tabela será exibida por padrão, assim como ocorre com outros parâmetros como <span class="highlighted-text">`quietly`</span> e <span class="highlighted-text">`mark_br`</span>.

## 2.4 Estratégias Mistas

<div id="botoesExemplo">
  <button id="botao-exemplo1" class="botao-interativo-exemplo" onclick="showConteudoExemplo('exemplo1')">Solução por br_plot</button>
  <button id="botao-exemplo2" class="botao-interativo-exemplo" onclick="showConteudoExemplo('exemplo2')">Payoff por função</button>
  <button id="botao-exemplo3" class="botao-interativo-exemplo" onclick="showConteudoExemplo('exemplo3')">Precisão do br_plot</button>
</div>

<script>
function showConteudoExemplo(conteudoId) {
  var conteudos = document.getElementsByClassName('conteudoExemplo');
  for (var i = 0; i < conteudos.length; i++) {
    conteudos[i].style.opacity = 0; // Definir a opacidade do conteúdo como 0 (invisível)
    conteudos[i].style.display = 'none'; // Esconder o conteúdo (display: none)
  }
  
  // Exibir o conteúdo desejado com animação suave
  var conteudoDesejado = document.getElementById(conteudoId);
  conteudoDesejado.style.display = 'block'; // Exibir o conteúdo (display: block)
  setTimeout(function() {
    conteudoDesejado.style.opacity = 1; // Definir a opacidade do conteúdo como 1 (visível)
  }, 50); // Aguardar 50 milissegundos para aplicar a opacidade (ajuste conforme desejado)

  // Remover a classe 'selecionadoExemplo' de todos os botões
  var botoes = document.getElementsByClassName('botao-interativo-exemplo');
  for (var i = 0; i < botoes.length; i++) {
    botoes[i].classList.remove('selecionadoExemplo');
  }

  // Adicionar a classe 'selecionadoExemplo' apenas ao botão clicado
  var botaoSelecionado = document.getElementById('botao-' + conteudoId);
  botaoSelecionado.classList.add('selecionadoExemplo');
}
</script>

<div id="exemplo1" class="conteudoExemplo">

Imagine um cenário onde o Governo e uma pessoa pobre estão interagindo. O Governo tem duas estratégias possíveis: "Ajuda" e "Não Ajuda", enquanto a pessoa pobre tem duas estratégias: "Trabalha" e "Vadia".


```r
jogo4 <- normal_form(
          players = c("Governo", "Pobre"),
          s1 = c("Ajuda", "Não Ajuda"), 
          s2 = c("Trabalha", "Vadia"), 
          payoffs1 = c(3, -1, -1, 0), 
          payoffs2 = c(2, 1, 3, 0))
```

Nesse jogo, o Governo e o pobre podem optar por estratégias puras, ou seja, escolher uma ação específica com certeza, ou podem utilizar estratégias mistas, ou seja, fazer escolhas ponderadas com base em probabilidades. Através do parâmetro <span class="highlighted-text">`mixed = TRUE`</span>, podemos encontrar a solução para esse tipo de jogo para as estratégias mistas.


```r
s_jogo4 <- solve_nfg(jogo4, 
                     mixed = TRUE, 
                     show_table = FALSE)
```

```
Pure strategy NE does not exist.
```

```
Mixed-strategy NE: [(1/2, 1/2), (1/5, 4/5)]
```

```
The obtained mixed-strategy NE might be only a part of the solutions.
```

```
Please examine br_plot (best response plot) carefully.
```

Com a função <span class="highlighted-text">`br_plot`</span> (*best response plot*) ou gráfico de melhores respostas é possível se chegar a maximização do payoff esperado.


```r
s_jogo4$br_plot
```

<img src="/rproject/pt-br/post/2023-09-22-jogos-simultaneos-tj/jogos-simultaneos_files/figure-html/mixedgraphgame-1.png" width="672" style="display: block; margin: auto;" />

</div>

<div id="exemplo2" class="conteudoExemplo">

A função de payoff em estratégias mistas é usada para representar o resultado esperado que um jogador obtém ao escolher uma determinada ação com probabilidade ou com incerteza. No exemplo a seguir estão as funções de payoff para as estratégias dos jogadores A e B.


- <p>Jogador: <span style="font-size: 80%;">$\{A, B\}$</span></p>


- <p>Estratégia: <span style="font-size: 80%;">$\{x \in [0, 1], y \in [0, 1] \}$</span></p>


- <p>Payoff: <span style="font-size: 80%;">$\{f_x(x, y) = -3x^2 + (1 - y) \times 2x, f_y(x, y) = -2y^2 + (1 - x) \times 3y\}$</span></p>


Desse modo, é possível definir um jogo fornecendo as funções de payoff como vetores de caracteres usando a função <span class="highlighted-text">`normal_form()`</span>. Nos parâmetros <span class="highlighted-text">`par1_lim`</span> e <span class="highlighted-text">`par2_lim`</span> estão configurados os valores entre 0 e 1, o que significa que as estratégias ou escolhas dos jogadores "A" e "B" estão limitadas a esse intervalo. Esses argumentos definem os limites para os eixos, no caso, <span class="highlighted-text">`"x"`</span> e <span class="highlighted-text">`"y"`</span> contidas em <span class="highlighted-text">`pars`</span>.


```r
jogo5 <- normal_form(
          players = c("A", "B"),
          payoffs1 = "-4*x^2 + (1-y) * 2*x",
          payoffs2 = "-2*y^2 + (1-x) * 3*y",
          par1_lim = c(0, 1),
          par2_lim = c(0, 1),
          pars = c("x", "y"))
```

Com a construção da estrutura do jogo na forma normal, temos a solução dele a apartir da função <span class="highlighted-text">`solve_nfg()`</span>.


```r
s_jogo5 <- solve_nfg(jogo5)
```

```
approximated NE: (0.1, 0.7)
```

```
The obtained NE might be only a part of the solutions.
Please examine br_plot (best response plot) carefully.
```

<img src="/rproject/pt-br/post/2023-09-22-jogos-simultaneos-tj/jogos-simultaneos_files/figure-html/game4b-1.png" width="672" style="display: block; margin: auto;" />

</div>

<div id="exemplo3" class="conteudoExemplo">

Nesse método alternativo, é possível definir os resultados de um jogo na forma normal, utilizando <span class="highlighted-text">`function`</span> da linguagem R


- <p>Jogador: <span style="font-size: 80%;">$\{A, B\}$</span></p>


- <p>Estratégia: <span style="font-size: 80%;">$\{x \in [0, 1], y \in [0, 1] \}$</span></p>


- <p>Payoff: <span style="font-size: 80%;">$\{f_x(x, y) = -nx^a + (b - y) \times px, f_y(x, y) = -my^s + (t - x) \times qy\}$</span></p>

<div style="margin-top: 0.5cm;"></div>


```r
# Função do jogador 1
f_x <- function(x, y, a, b, n, p) {
          -n*x^a + (b - y) * p*x
        }

# Função do jogador 2
f_y <- function(x, y, s, t, m, q) {
          -m*y^s + (t - x) * q*y
        }
```

Após a definição das funções para os jogadores 1 e 2, podemos incorporá-las dentro da <span class="highlighted-text">`normal_form()`</span> nos parâmetros <span class="highlighted-text">`payoffs1`</span> e <span class="highlighted-text">`payoffs2`</span>. 


```r
jogo6 <- normal_form(
          players = c("A", "B"),
          payoffs1 = f_x,
          payoffs2 = f_y,
          par1_lim = c(0, 1),
          par2_lim = c(0, 1),
          pars = c("x", "y"))
```

Para obter uma solução aproximada numericamente usando o `solve_nfg()`, é necessário definir os valores dos parâmetros da função que devem ser tratados como constantes, utilizando os argumentos <span class="highlighted-text">`cons1`</span> e <span class="highlighted-text">`cons2`</span>, ambos aceitando uma lista de nomes. Além disso, é possível evitar que o gráfico das melhores respostas seja exibido, definindo <span class="highlighted-text">`plot = FALSE`</span>.


```r
spf_jogo6 <- solve_nfg(
              game = jogo6,
              cons1 = list(
                a = 2, b = 1, n = 4, p = 2
                ),
              cons2 = list(
                s = 2, t = 1, m = 2, q = 3
                ),
              plot = FALSE)
## approximated NE: (0.1, 0.7)
## The obtained NE might be only a part of the solutions.
## Please examine br_plot (best response plot) carefully.
```

 É possível ajustar a precisão da aproximação através do parâmetro <span class="highlighted-text">`precision`</span> (por padrão, <span class="highlighted-text">`precision = 1`</span>). Ao aumentar o valor, o cálculo das soluções será feito com maior detalhamento, resultando em uma aproximação mais precisa dos resultados. No entanto, é importante notar que quanto maior o valor desse parâmetro, mais tempo o processo de cálculo pode levar, especialmente para jogos mais complexos com muitas estratégias e jogadores. Portanto, é recomendado ajustar o valor da precisão de acordo com a complexidade do jogo e a necessidade de precisão dos resultados.
 

```r
s_jogo6 <- solve_nfg(
            game = jogo6,
            cons1 = list(
              a = 2, b = 1, n = 4, p = 2
              ),
            cons2 = list(
              s = 2, t = 1, m = 2, q = 3
              ),
            precision = 3)
```

```
approximated NE: (0.077, 0.692)
```

```
The obtained NE might be only a part of the solutions.
Please examine br_plot (best response plot) carefully.
```

Para extrair o gráfico das melhores respostas com o ponto de Equilíbrio de Nash (<span class="highlighted-text">`NE`</span>) marcado, é possível utilizar a função <span class="highlighted-text">`br_plot_NE`</span>. Esse gráfico mostra como as estratégias dos jogadores podem mudar e se adaptar para alcançar o <span class="highlighted-text">`NE`</span>.


```r
s_jogo6$br_plot_NE
```

<img src="/rproject/pt-br/post/2023-09-22-jogos-simultaneos-tj/jogos-simultaneos_files/figure-html/game6d-1.png" width="672" style="display: block; margin: auto;" />

</div>

Com estratégias mistas, os jogadores podem tomar decisões aleatórias com diferentes probabilidades para cada ação. Isso torna o resultado do jogo mais incerto, pois o desfecho dependerá das escolhas aleatórias feitas por cada jogador.

# 3. Referências

HAZRA, Tanmoy; ANJARIA, Kushal. Applications of game theory in deep learning: a survey. **Multimedia Tools and Applications**, v. 81, n. 6, p. 8963-8994, 2022. DOI: <a href="https://doi.org/10.1007/s11042-022-12153-2" target="_blank" style="color:#016dea; text-decoration: none;" onmouseover="this.style.color='#014ba0';" onmouseout="this.style.color='#016dea';">https://doi.org/10.1007/s11042-022-12153-2</a>.

MANKIW, N. Gregory et al. **Introdução à economia**. 2005.

YANAI, Yusei; KAMIJO, Yoshio. **Game Theory With R**. Shin-Ogawacho, Shinjuku-ku, Tóquio, JP: Asakura Publishing Co,. Ltd., 2023. 244 p. ISBN 978-4-254-27024-2 C3050.
