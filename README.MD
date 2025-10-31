# 📚 README - Problema da Dieta de Stigler

# 🎯 Sobre o Projeto
Implementação do famoso "Problema da Dieta de Stigler" (1939) usando Programação Linear com Google OR-Tools. Este foi o primeiro problema de otimização linear resolvido computacionalmente.

# ❓ Perguntas Frequentes e Respostas
# 1. "Por que o custo da nossa solução (R$ 261,51/ano) é diferente do Stigler original (US$ 39,93/ano)?"
Resposta: Há várias razões para essa diferença:

Problema de Escala nos Dados: Os valores nutricionais nos CSVs originais parecem estar em uma escala incorreta. Por exemplo, "44.7 calorias para 10lb de farinha" é unrealisticamente baixo.

Conversão de Moeda: Estamos trabalhando em Reais, enquanto Stigler usou Dólares de 1939.

Inflação: US$ 39,93 em 1939 equivalem a aproximadamente US$ 900 hoje.

Metodologia Diferente: Stigler usou métodos manuais, nós usamos otimização computacional.

Correção Sugerida: Multiplicar todos os valores nutricionais por ~100 nos cálculos.

# 2. "Por que o algoritmo escolhe apenas 4 alimentos? Isso é saudável?"
Resposta: Não, não é saudável! Isso acontece porque:

Otimização Pura: O algoritmo encontra a solução MATEMATICAMENTE ótima, não nutricionalmente balanceada

Eficiência de Custo: Navy Beans, Corn Meal, Cabbage e Spinach oferecem a melhor relação nutriente/custo

Falta de Restrições de Variedade: Não há restrições que forcem diversidade alimentar

Para uso real: Deveríamos adicionar restrições de variedade mínima e limites máximos por alimento.

# 3. "Qual algoritmo de otimização está sendo usado e por quê?"
Resposta: Usamos o Método Simplex através do solver GLOP do Google OR-Tools porque:

GLOP (Google Linear Optimization Package) é especializado em problemas de Programação Linear contínua

Simplex é eficiente para problemas com até milhares de variáveis e restrições

Convergência Garantida: Para problemas convexos de PL, encontra sempre o ótimo global

Open Source: Diferente de solvers comerciais como CPLEX ou Gurobi

# 4. "Por que usar variáveis contínuas (NumVar) em vez de inteiras (IntVar)?"
Resposta: Escolhemos variáveis contínuas porque:

Flexibilidade: Permite comprar frações de gramas (mais realista para alimentos)

Eficiência Computacional: Problemas contínuos convergem mais rápido

Precisão: Evita erros de arredondamento em quantidades pequenas

Para implementação prática: Em um sistema real, converteríamos para IntVar após a otimização contínua.

# 5. "O modelo considera sazonalidade e variação de preços?"
Resposta: Não, o modelo atual tem limitações:

Preços Fixos: Usa apenas os preços de 1939 fornecidos nos dados originais

Sem Sazonalidade: Não considera que preços mudam ao longo do ano

Disponibilidade Constante: Assume que todos alimentos estão sempre disponíveis

Extensão possível: Poderíamos criar múltiplos cenários com diferentes conjuntos de preços.

# 6. "Como lidar com restrições adicionais como alergias ou preferências?"
    Resposta: O modelo pode ser facilmente extendido:

    # Excluir amendoim para alérgicos
    solver.Add(variaveis['Peanut Butter'] == 0)

    # Limitar quantidade máxima de um alimento
    solver.Add(variaveis['Liver (Beef)'] <= 50)  # Máximo 50g

    # Forçar variedade mínima
    solver.Add(sum([var > 0 for var in variaveis.values()]) >= 8)

# 7. "A solução é viável para implementação real?"
Resposta: Como está, NÃO, mas serve como base para:

Protótipo Educacional: Excelente para aprender otimização linear

Base para Sistemas Reais: Com adaptações pode ser usado em:

Planejamento de cardápios institucionais

Otimização de compras para restaurantes

Sistemas de recomendação nutricional

Adaptações necessárias:

Dados nutricionais atualizados

Preços atuais e sazonais

Restrições de palatabilidade e variedade

Considerações culturais e religiosas

# 8. "Como escalar para uma família em vez de indivíduo?"

    Resposta: Simplesmente multiplicando os requisitos:
    # Para família de 4 pessoas

    tamanho_familia = 4
    for nutriente in nutrientes:
        minimo_original = nutriente['minimo']
        nutriente['minimo'] = minimo_original * tamanho_familia

# 9. "E se não houver solução viável?"
Resposta: O solver retornará status = INFEASIBLE. Nesse caso:

Relaxar Restrições: Reduzir alguns requisitos nutricionais

Adicionar Alimentos: Incluir mais opções na base de dados

Permitir Déficits: Aceitar que alguns nutrientes fiquem abaixo do mínimo

# 10. "Quais as principais limitações deste modelo?"
Resposta: Limitações importantes:

Dados Históricos: Preços e valores nutricionais de 1939

Simplicidade Nutricional: Considera apenas 9 nutrientes, ignora outros importantes

Ausência de Preferências: Não considera gosto, hábitos culturais ou textura

Preços Fixos: Não captura flutuações de mercado

Otimização Extrema: Pode sugerir dietas monótonas e não-saudáveis

🚀 Como Executar
    # Instalar dependências
    pip install pandas ortools

    # Executar o programa
    python dieta_otimizada.py

📊 Estrutura dos Arquivos
    stigler_diet/
    ├── dieta_otimizada.py          # Código principal
    ├── nutrientes.csv              # Requisitos nutricionais
    ├── data.csv                    # Dados dos alimentos
    ├── README.md                   # Este arquivo
    └── requirements.txt            # Dependências