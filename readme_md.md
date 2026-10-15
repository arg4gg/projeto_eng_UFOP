# 📊 GestãoInteligente

> **Automação comercial e apoio à decisão para pequenas e médias lojas de materiais de construção.**

![Versão Documental](https://img.shields.io/badge/Vers%C3%A3o%20SRS-4.0-blue)
![Engenharia de Software](https://img.shields.io/badge/UFOP-DECOM-red)
![Padrão](https://img.shields.io/badge/Padr%C3%A3o-IEEE%20830--1998-green)
![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow)

---

## 📌 1. Visão Geral do Projeto

O **GestãoInteligente** é um sistema corporativo (SaaS) voltado para otimizar a operação no balcão, no armazém e no escritório de lojas de materiais de construção (foco inicial na região dos Inconfidentes - MG). 

O sistema resolve problemas clássicos do setor, como capital imobilizado em estoque de baixo giro, lentidão nas cotações via WhatsApp, falta de histórico padronizado de movimentações e dependência de planilhas informais.

---

## 🏗️ 2. Arquitetura do Produto

O produto é estruturado em **três componentes centrais**:

```
 ┌─────────────────────────────────────────────────────────┐
 │                   GestãoInteligente                     │
 ├───────────────────┬─────────────────┬───────────────────┤
 │  GestãoInteligente │ GestãoInteligente│ GestãoInteligente │
 │        Core       │       Loja      │       Comm        │
 └─────────┬─────────┴────────┬────────┴─────────┬─────────┘
           │                  │                  │
   [Servidor Central /   [App Desktop /     [Módulo de    
    Dados & Inteligência]  Web Balcão]      Comunicação]  
```

1. **GestãoInteligente Core:** Servidor central que consolida os dados da loja, executa regras financeiras/estoque e processa os motores de inteligência e relatórios.
2. **GestãoInteligente Loja:** Aplicação client-side utilizada por vendedores, estoquistas e gestores no balcão, armazém e escritório.
3. **GestãoInteligente Comm:** Módulo responsável pelo disparo automatizado de orçamentos, avisos e relatórios via WhatsApp e E-mail, com fila de reenvio assíncrono em caso de falhas.

---

## 🎯 3. Missão e Indicadores de Desempenho (KPIs)

A eficácia da implantação do sistema é avaliada 6 meses após o uso com base em 3 metas claras:

| Indicador | Meta | Descrição |
| :--- | :---: | :--- |
| **Tempo de Orçamento** | **-60%** | Redução do tempo médio entre o pedido de cotação e a entrega em PDF ao cliente. |
| **Acurácia de Inventário** | **≥ 98%** | Porcentagem de itens sem divergência entre saldo sistêmico e contagem física mensal. |
| **Capital Imobilizado** | **-25%** | Redução do valor estocado em produtos da Classe C sem venda há mais de 90 dias. |

---

## 🛑 4. Escopo e Limites do Sistema

Para manter o foco nas dores operacionais da loja, os **limites da versão atual** são:

* 🚫 **Não emite Nota Fiscal Eletrônica (NF-e):** Os dados de vendas/lançamentos são exportados em CSV padronizado para o escritório de contabilidade parceiro.
* 🚫 **Não processa pagamentos diretamente:** Registra formas de pagamento e contas a receber, mas não atua como gateway/maquininha/PIX.
* 🚫 **Não é e-commerce/loja virtual:** O foco é o atendimento assistido pelo vendedor.
* 🚫 **Não substitui a contagem física:** Perdas, quebras e avarias demandam lançamento manual justificável.

---

## 👥 5. Hierarquia de Atores e Permissões

O sistema adota **Controle de Acesso Baseado em Perfis (RBAC)** validado no servidor:

```
          [Usuário do Sistema] (Ator Abstrato)
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
    [Vendedor]          [Estoquista]
         └─────────┬─────────┘
                   ▼
            [Gestor da Loja]
                   │
                   ▼
      [Administrador do Sistema]
```

### Matriz de Acesso Resumida

| Operação | Vendedor | Estoquista | Gestor | Admin |
| :--- | :---: | :---: | :---: | :---: |
| Consultar estoque e cadastros | ✅ | ✅ | ✅ | ✅ |
| Cadastrar clientes (com aceite LGPD) | ✅ | ❌ | ✅ | ✅ |
| Cadastrar/Alterar produtos e fornecedores | ❌ | ✅ | ✅ | ✅ |
| Registrar movimentações de estoque | 🔍 *Consulta* | ✅ | ✅ | ✅ |
| Montar orçamentos e registrar/cancelar vendas | ✅ | ❌ | ✅ | ✅ |
| Consultar/Tratar alertas financeiros | ❌ | ❌ | ✅ | ✅ |
| Acessar relatórios, curva ABC e DRE | ❌ | ❌ | ✅ | ✅ |
| Gestão de usuários, backups e auditoria | ❌ | ❌ | ❌ | ✅ |

---

## 🧰 6. Serviços Oferecidos

1. **Autenticação:** Login por e-mail/senha com bloqueio de segurança após 5 tentativas mal-sucedidas.
2. **Controle de Acesso:** Gerenciamento de papéis e permissões dinâmicas.
3. **Cadastros:** Controle de produtos (EAN/código de barras, unidade, estoque mínimo, preços), clientes e fornecedores.
4. **Gestão de Estoque:** Movimentações rastreáveis (Entrada, Saída, Perda, Ajuste com justificativa).
5. **Vendas e Orçamentos:** Montagem de cotações com desconto, validade, geração instantânea de PDF e conversão direta em venda.
6. **Comunicação Automatizada:** Disparos de PDFs por WhatsApp API e e-mail via filas resilientes.
7. **Alertas:** Avisos em tempo real de estoque mínimo, itens sem giro (90 dias) e contas a vencer.
8. **Gestão Financeira:** Lançamento/baixa de contas a pagar/receber e exportação contábil.
9. **Relatórios e Inteligência:** Fluxo de caixa, balanço, curva ABC (80-15-5) e projeção de demanda sazonal.
10. **Manutenção e Auditoria:** Backups programados, verificação de integridade e log imutável de audit trail.

---

## 🚀 7. Roadmap & Planejamento de Sprints

### Backlog de Requisitos Funcionais (Priorizado)

| ID | Requisito Funcional | Serviço | Est. (Pontos) | Sprint |
| :---: | :--- | :--- | :---: | :---: |
| **RF10** | Autenticação e encerramento de sessão | Autenticação | 2 | **Sprint 1** |
| **RF06** | Gestão de acesso e perfis de usuário | Controle de Acesso | 3 | **Sprint 1** |
| **RF07** | Cadastros de produtos, clientes e fornecedores | Cadastros | 3 | **Sprint 1** |
| **RF01** | Controle e movimentação rastreável de estoque | Gestão de Estoque | 8 | **Sprint 1** |
| **RF03** | Geração e envio automatizado de orçamentos | Vendas / Comunicação | 8 | Sprint 2 |
| **RF08** | Registro e cancelamento de vendas | Vendas e Orçamentos | 5 | Sprint 2 |
| **RF04** | Fluxo de caixa, balanço e curva ABC | Relatórios | 8 | Sprint 3 |
| **RF02** | Alertas de reposição e de sazonalidade | Alertas | 5 | Sprint 3 |
| **RF05** | Contas a pagar e a receber | Gestão Financeira | 5 | Sprint 3 |
| **RF09** | Backup, restauração e trilha de auditoria | Manutenção/Auditoria | 3 | Sprint 4 |

---

## 🧪 8. Exemplo de Cenário de Aceitação (BDD)

```gherkin
Cenário: RF01 - Ajuste de estoque sem justificativa é rejeitado
  Dado que o produto "Cimento CP-II 50 kg" tem saldo de 40 sacos
  Quando o estoquista registra um AJUSTE para 38 sacos sem informar o motivo
  Então o sistema rejeita a operação
  E exibe a mensagem "Informe o motivo do ajuste"
  E mantém o saldo inalterado em 40 sacos

Cenário: RF06 - Restrição de acesso a relatórios sensíveis
  Dado um usuário autenticado com o perfil Vendedor
  Quando ele tenta acessar o relatório de Fluxo de Caixa
  Então o acesso é negado
  E a tentativa não autorizada é registrada na trilha de auditoria
```

---

## ⚡ 9. Requisitos Não Funcionais (Métricas de Aceitação)

* **RNF01 (Usabilidade):** Vendedor treinado deve conseguir montar um orçamento completo em no máximo 2 horas de capacitação.
* **RNF02 (Confiabilidade):** Disponibilidade do servidor $\ge 99,5\%$ ao mês.
* **RNF04 (Desempenho - Busca):** Retorno de busca de produtos em $< 500\text{ ms}$ para base com até 100.000 itens.
* **RNF05 (Desempenho - PDF):** Geração do PDF do orçamento em $\le 2\text{ s}$.
* **RNF06 (Desempenho - Relatórios):** Processamento do relatório anual em $\le 10\text{ s}$.
* **RNF07 (Segurança):** Bloqueio por 15 minutos ao atingir 5 tentativas consecutivas de senha incorreta.

---

## 📄 10. Referências e Créditos

* **Universidade Federal de Ouro Preto (UFOP)** — Departamento de Computação (DECOM)
* **Disciplina:** Engenharia de Software I
* **Docente responsável:** Prof. Tiago Garcia de Senna Carneiro
* **Norma Padrão:** IEEE Std 830-1998 (Especificação de Requisitos de Software)
* **Template e Metodologia:** TerraLAB / UFOP