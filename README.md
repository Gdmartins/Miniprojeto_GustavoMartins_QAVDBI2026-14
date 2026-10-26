# Miniprojeto_GustavoMartins_QAVDBI2026-14
Mini Projeto — QAVDBI2026/14
**Autor:** Gustavo Martins  
**Arquivo:** `README_GustavoMartins_QAVDBI2026/14.md`  
**Origem:** Colab notebook (link fornecido pelo autor)Este repositório contém um **mini projeto** desenvolvido em um notebook Google Colab. O objetivo do projeto é demonstrar um fluxo de trabalho simples de análise/experimentos (pré-processamento, modelagem, visualização e avaliação) conforme implementado no notebook original.
## Estrutura do projeto
- `Notebook Colab` — código principal e explicações passo a passo (células executáveis).
- `README_GustavoMartins_QAVDBI2026/14.md` — este arquivo com instruções e histórico de commits demonstrativo.

> Observação: o código-fonte principal está no notebook Colab. Para rodar localmente, exporte o notebook como `.py` ou execute no Colab.
## Requisitos
- Conta Google para abrir o Colab (opcional, mas recomendado).
- Se preferir rodar localmente:
  - Python 3.8+ instalado
  - Dependências listadas no notebook (ex.: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `tensorflow`/`torch` se houver)
  - Recomenda-se criar um ambiente virtual: `python -m venv .venv && source .venv/bin/activate` (Linux/Mac) ou `.venv\Scripts\activate` (Windows)

## Instruções rápidas
### Executar no Colab (recomendado)
1. Abra o link do notebook no Google Colab.
2. No menu, selecione **Runtime > Run all** (ou clique em "Executar tudo") para executar todas as células sequencialmente.
3. Acompanhe as saídas e gráficos diretamente no Colab.

### Executar localmente (opcional)
1. Exporte o notebook como script Python (`File > Download > Download .py`) ou clone o repositório se houver.
2. Instale dependências (ex.: `pip install -r requirements.txt`) — se o arquivo `requirements.txt` não existir, instale manualmente as bibliotecas usadas no notebook.
3. Rode o script:
   ```bash
   python Miniprojeto_GustavoMartins_QAVDBI2026_14.py .


commit 1 — init: Adicionado notebook base com esqueleto do projeto e seções principais.

commit 2 — data: Inclusão do carregamento de dados e primeiras rotinas de limpeza.

commit 3 — features: Engenharia de features e transformações básicas.

commit 4 — model: Implementação do modelo inicial e rotina de treino.

commit 5 — eval: Métricas de avaliação e visualizações básicas.

commit 6 — docs: Adição de README e comentários explicativos no notebook.

**commit final** —release`: Ajustes finais e verificação de execução completa no Colab.

### Segue Link do arquivo com a lógica desenvolvida 

https://colab.research.google.com/drive/1gYkbCAC4JUWqND_FUxr3ecFG9q04OIPe?usp=sharing

### Segue Link do repositório principal do projeto



## Código estrutural dos dados com a resolução

### Exemplo 1: vendas por categoria (soma de valor)
    # Identificar colunas de categoria e valor
    cat_cols = [c for c in df.columns if 'categoria' in c.lower() or 'cat' in c.lower() or 'segmento' in c.lower() or 'categoria_produto' in c.lower()]
    valor_cols = [c for c in df.columns if any(k in c.lower() for k in ['valor','preco','amount','price','total'])]
    print("Colunas de categoria detectadas:", cat_cols)
    print("Colunas de valor detectadas:", valor_cols)

    if cat_cols and valor_cols:
        cat = cat_cols[0]
        val = valor_cols[0]
        grp = df.groupby(cat)[val].agg(['sum','count','mean']).sort_values('sum', ascending=False)
        print(f"\nVendas por {cat} (soma, contagem, média):")
        print(grp.head(10))
    else:
        print("Não foi possível identificar colunas de categoria/valor automaticamente para o agrupamento 1.")

    # Exemplo 2: clientes que mais compram (por cliente_id ou nome)
    cliente_cols = [c for c in df.columns if 'cliente' in c.lower() or 'cliente_id' in c.lower() or 'customer' in c.lower()]
    if cliente_cols and valor_cols:
        cliente = cliente_cols[0]
        val = valor_cols[0]
        top_clientes = df.groupby(cliente)[val].agg(['sum','count']).sort_values('sum', ascending=False).head(10)
        print(f"\nTop 10 clientes por valor total de compras (coluna usada: {cliente}):")
        print(top_clientes)
    else:
        print("Não foi possível identificar colunas de cliente/valor automaticamente para o agrupamento 2.")

    # Exemplo 3 (opcional): vendas por gênero (se existir) e por mês
    genero_cols = [c for c in df.columns if 'genero' in c.lower() or 'sexo' in c.lower()]
    data_cols = [c for c in df.columns if df[c].dtype == 'datetime64[ns]']
    if genero_cols and valor_cols:
        g = genero_cols[0]
        v = valor_cols[0]
        grp_gen = df.groupby(g)[v].agg(['sum','count']).sort_values('sum', ascending=False)
        print(f"\nVendas por {g}:")
        print(grp_gen)
    else:
        print("Coluna de gênero não detectada automaticamente.")

    if data_cols and valor_cols:
        d = data_cols[0]
        v = valor_cols[0]
        df['ano_mes'] = df[d].dt.to_period('M')
        vendas_mes = df.groupby('ano_mes')[v].sum().sort_index()
        print("\nVendas por mês (soma):")
        print(vendas_mes.head(12))
    else:
        print("Coluna de data em formato datetime não detectada para análise temporal.")

def conclusoes_e_proximos_passos(df_original, df_limpo):
    """Produz um bloco de conclusões (3-6 tópicos)."""
    print("\n=== Conclusões e próximos passos ===")
    conclusoes = [
        "1) Qualidade inicial: a base apresenta nulos e duplicatas; é necessário revisar colunas essenciais antes de análises críticas.",
        "2) Limpeza aplicada: removemos linhas com nulos em colunas essenciais e imputamos número de filhos com mediana; essa escolha preserva amostra e reduz viés por outliers.",
        "3) Insights operacionais iniciais: categorias X (ver output de agrupamento) concentram a maior parte do faturamento; poucos clientes respondem por grande parcela das vendas (efeito Pareto provável).",
        "4) Problemas remanescentes: possíveis registros com datas inválidas convertidas para NaT; valores negativos ou outliers em colunas de valor precisam de investigação manual.",
        "5) Próximos passos recomendados: validar chaves (IDs de cliente/produto), enriquecer dados demográficos, e criar dashboards com séries temporais e cohort analysis para entender retenção."
    ]
