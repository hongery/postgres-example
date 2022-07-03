# postgres-example


## migration
brew install migration
```
up/down migration
old db schema > migrate up > x.up.sql > new db scheme
new db schema > migrate down > x.down.sql > old db schema
# up , check db simple_bank table 
migrate -path db/migration -database "postgresql://root:secret@localhost:5432/simple_bank?sslmode=disable" -verbose up 

```

## db库的选择
1. database/sql
    * 优点：fast & straightforward，Manual mapping sql filelds to variables
    * 缺点：easy to make mistakes,not caught util runtime
2. Gorm
    * 优点：CRUD func already implemented, very short production code,Must learn to write queried using gorm's function 
    * 缺点：Run slowly on high load
3. SQLX
    * 优点：Quite fast & easy to use,Fileds mapping via query text & struct tags
    * 缺点：Failure won't ovvur until runtime
4. SQLC
    * 优点：Very fast & easy to use,Autoatic code generation.Catch SQL query errors before generating codes
    * 缺点：Full support Postgres. Mysql is experimental

这里选择sqlc
```
brew install kyleconroy/sqlc/sqlc  # github.com/kyleconriy/sqlc
# check version 
sqlc version
# config 
version: "1"
packages:
  - name: "db"
    path: "./db/sqlc"
    queries: "./db/query/"
    schema: "./db/migration/"
    engine: "postgresql"
    emit_json_tags: true
    emit_prepared_queries: false
    emit_interface: true
    emit_exact_table_names: false
    emit_empty_slices: true
# table 生成golang代码
Accoutn table > Accoutn struct
```

## Mock db

```
# install mockdb
go install github.com/golang/mock/mockgen@v1.6.0

# get 依赖库
go get github.com/golang/mock/gomock

# make
make mock
```