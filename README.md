# Porsche Sales Intelligence

Dashboard estática para explorar vendas de Porsche nos EUA. A interface e a lógica ficam em `index.html`; os estilos estão separados em `style.css`. Os dados tratados também estão disponíveis em CSV e Excel.

## Como executar

Abra `index.html` em um navegador. Não há servidor ou processo de compilação. Fontes e Chart.js são carregados de CDNs, então esses recursos externos dependem de conexão com a internet; os dados e o assistente funcionam localmente no navegador.

## O que o painel mostra

1. **Modelos por cidade:** compara vendas ou receita para entender o mix de modelos.
2. **Ano-modelo:** mostra a distribuição das vendas por ano-modelo.
3. **Vendas por estado:** compara o número de vendas e a receita.

Os gráficos de apoio mostram forma de pagamento e evolução mensal da receita. A leitura executiva, os seis indicadores e a tabela são atualizados conforme os filtros.

### Filtros e indicadores

Os sete filtros podem ser combinados: modelo, ano-modelo, ano da venda, estado, cidade, pagamento e etapa de entrega. Os indicadores mostram vendas registradas, receita, ticket médio, modelo líder, ano-modelo dominante e taxa de entrega.

Vendas canceladas contam como registros, mas não entram na receita nem no ticket médio. A evolução mensal considera apenas vendas não canceladas com data válida.

### Assistente

O assistente interpreta perguntas e comandos em português por meio de regras locais em JavaScript. Por exemplo: “top estados por receita”, “ticket médio do Macan” e “filtre Taycan em 2025”. Ele não chama um serviço de IA nem envia dados para uma API.

## Organização do código

### `index.html`

- **Cabeçalho e conteúdo semântico:** título, filtros, indicadores, cartões dos gráficos, recomendações, tabela e assistente.
- **Base `D`:** registros usados pelo painel, embutidos para dispensar um backend. Cada objeto usa estas chaves:

  | Chave | Significado |
  |---|---|
  | `id` | Identificador da venda |
  | `d` | Data da venda; `null` representa data inválida ou ausente |
  | `m` | Modelo Porsche |
  | `f` | Família do modelo |
  | `y` | Ano-modelo |
  | `p` | Preço em dólares |
  | `mi` | Quilometragem em milhas |
  | `pay` | Forma de pagamento |
  | `c` | Cidade |
  | `s` | Sigla do estado |
  | `sp` | Vendedor |
  | `sg` | Etapa/status de entrega |
  | `yr` | Ano da venda, derivado de `d` durante a inicialização |

- **Funções auxiliares:** normalizam texto, formatam valores, agregam vendas e aplicam filtros.
- **`render()`:** recalcula os indicadores, desenha os gráficos, gera recomendações e atualiza tabela e rodapé.
- **Assistente:** reconhece dimensões, estados, status, pagamentos e comandos, respondendo usando a base local.
- **Canvas:** desenha a ilustração do carro e o favicon. A animação respeita a preferência por movimento reduzido do sistema.

Os comentários junto às seções do HTML e do JavaScript explicam o papel de cada parte e as regras de cálculo.

### `style.css`

Define as cores e tipografia, a aparência dos cartões, gráficos, filtros, tabela e janela do assistente. Os `@media` reorganizam o layout para tablets e celulares; a última regra considera a preferência por movimento reduzido.

### Arquivos de dados

- `vendas_porsche_tratado.csv`: base tabular tratada, que pode ser aberta em uma planilha ou ferramenta de análise.
- `vendas_porsche_tratado.xlsx`: a mesma base em formato Excel.

O CSV contém as colunas `sale_id`, `sale_date`, `date_quality`, `porsche_model`, `model_family`, `model_year`, `sale_price_usd`, `vehicle_mileage_mi`, `payment_method`, `city`, `state`, `salesperson`, `delivery_status`, `delivery_stage`, `sale_year` e `sale_month`. O painel usa uma versão compacta dos campos na constante `D`; os dados tabulares não recebem comentários dentro dos arquivos para preservar sua estrutura.

## Tratamento da base

| Campo | Tratamento |
|---|---|
| Datas | Datas inválidas ou inexistentes ficam sem data; são exibidas como “Sem data” |
| Ano-modelo | Padronizado para número de quatro dígitos |
| Preço | Convertido para número em dólares |
| Quilometragem | Convertida para milhas |
| Pagamento, cidade, estado e entrega | Valores padronizados em categorias |
| Vendedores | Nomes incompletos associados ao vendedor correspondente quando possível |

Os nomes dos clientes não fazem parte da base embutida no HTML nem do CSV distribuído.

## Publicação

Para publicar como site estático (por exemplo, GitHub Pages), mantenha `index.html`, `style.css` e os arquivos que quiser disponibilizar juntos na raiz do site. Configure a publicação para servir a raiz do repositório. Não é necessário enviar a planilha ou o CSV para o dashboard funcionar: os dados usados pela página já estão na constante `D` em `index.html`.

## Estrutura do projeto

```text
index.html                     # interface, base embutida e lógica do dashboard
style.css                      # tema, componentes e regras responsivas
vendas_porsche_tratado.csv     # base tratada em formato tabular
vendas_porsche_tratado.xlsx    # base tratada em formato Excel
README.md                      # descrição do projeto e guia de leitura
```
