# AvaliacaoEngenheiroDadosCNI_Parquet
avaliacao engenheiro dados cni

Como gerar o Parquet localmente (passo a passo)

1 - Instalei as dependência:
pip install pyarrow pandas

## Depois fiz essa sequencia:

1. Chamar a função para carregar os dados.
dados_carregados = carregar_dados_csv(arquivo_csv_entrada)

2. Chamar a função para processar os dados.
dados_processados = processar_e_transformar_dados(dados_carregados)

3. Chamar a função para salvar os dados em Parquet.
salvar_dados_em_parquet(dados_processados, arquivo_parquet_saida)

print("\nProcesso de conversão finalizado.")

## Segue o script completo e 3 funções:

import pandas as pd

def carregar_dados_csv(caminho_arquivo):
    """
    Etapa 1: Carregar os dados de um arquivo CSV.

    Esta função lê um arquivo CSV do caminho especificado e o carrega em um
    DataFrame do pandas, que é uma estrutura de dados tabular otimizada
    para manipulação de dados em Python.

    Args:
        caminho_arquivo (str): O caminho para o arquivo CSV de entrada.

    Returns:
        pandas.DataFrame: Um DataFrame contendo os dados do arquivo.
    """
    print(f"Iniciando Etapa 1: Carregando dados de '{caminho_arquivo}'...")
    df = pd.read_csv(caminho_arquivo)
    print("Dados carregados com sucesso.")
    return df

def processar_e_transformar_dados(df):
    """
    Etapa 2: Realizar transformações e limpeza nos dados.

    Esta função executa várias operações para limpar e estruturar o DataFrame.
    Para este conjunto de dados, as seguintes transformações são aplicadas:
    1.  A conversão da coluna de data para um tipo de dado datetime,
        permitindo manipulações de data e hora.

    Args:
        df (pandas.DataFrame): O DataFrame original a ser transformado.

    Returns:
        pandas.DataFrame: O DataFrame após as transformações.
    """
    print("\nIniciando Etapa 2: Processando e transformando os dados...")
    # Converte a coluna 'DataLiberacao' para o formato datetime.
    # Isso é crucial para análises temporais e para garantir a compatibilidade
    # com o formato Parquet.
    if 'DataLiberacao' in df.columns:
        print("Convertendo a coluna 'DataLiberacao' para datetime...")
        df['DataLiberacao'] = pd.to_datetime(df['DataLiberacao'])
    print("Transformação de dados concluída.")
    return df

def salvar_dados_em_parquet(df, caminho_arquivo_saida):
    """
    Etapa 3: Salvar os dados processados em formato Parquet.

    Esta função recebe o DataFrame transformado e o salva no formato Parquet.
    Parquet é um formato de armazenamento colunar eficiente, ideal para
    análises de big data.

    **Nota Importante:** Para esta função rodar em um ambiente local, você
    precisará instalar uma das bibliotecas de engine do Parquet. Você pode
    fazer isso com o comando:
    `pip install pyarrow` ou `pip install fastparquet`

    Args:
        df (pandas.DataFrame): O DataFrame final a ser salvo.
        caminho_arquivo_saida (str): O nome do arquivo .parquet de saída.
    """
    print(f"\nIniciando Etapa 3: Salvando DataFrame em '{caminho_arquivo_saida}'...")
    try:
        # Tenta salvar em Parquet usando a biblioteca 'pyarrow'
        df.to_parquet(caminho_arquivo_saida, index=False, engine='pyarrow')
        print("Arquivo Parquet salvo com sucesso.")
    except ImportError:
        # Se a biblioteca não estiver instalada, informa o usuário e salva em CSV
        print("ERRO: A biblioteca 'pyarrow' não está instalada.")
        print("Por favor, instale-a com 'pip install pyarrow' para salvar em Parquet.")
        print("Como alternativa, salvarei o arquivo em formato CSV.")
        caminho_csv_alternativo = caminho_arquivo_saida.replace('.parquet', '.csv')
        df.to_csv(caminho_csv_alternativo, index=False)
        print(f"Arquivo salvo como '{caminho_csv_alternativo}'")

