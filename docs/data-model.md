\# Modelagem de Dados



\## Entidades



\### Pedido (`orders`)



| Coluna | Tipo | Descrição |

|---|---|---|

| id | String | Identificador único do pedido |

| customer | String | Nome do cliente |

| status | String | Status do pedido, com valor inicial `open` |

| created\_at | DateTime | Data e hora de criação do pedido |



\### Item (`items`)



| Coluna | Tipo | Descrição |

|---|---|---|

| id | String | Identificador único do item |

| order\_id | String | Identificador do pedido associado |

| sku | String | Código SKU do produto |

| description | String | Descrição do item |

| quantity | Integer | Quantidade do item |



\## Relacionamento



As entidades `orders` e `items` possuem relacionamento 1:N.



Um pedido pode possuir vários itens, enquanto cada item pertence a apenas um pedido.



A coluna `items.order\_id` é uma chave estrangeira que referencia `orders.id`.



\## Como as tabelas são criadas



A aplicação utiliza SQLAlchemy como ORM.



As entidades são definidas em `app/models.py` e as tabelas são criadas automaticamente na inicialização da aplicação por meio de `Base.metadata.create\_all(bind=engine)`.



A string de conexão é obtida da variável de ambiente `DATABASE\_URL`. Quando ela não está definida, a aplicação utiliza SQLite local como fallback.

