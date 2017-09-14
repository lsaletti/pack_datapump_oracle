# PACKAGE PACK_DATAPUMP

Criamos uma package dentro de tools para realizarmos import/export de maneira automática através do Flyway.
Está documentação exemplificará os principais casos de import e export mais utilizados no nosso dia-a-dia.

### Export tabela simples
```sh
BEGIN
    TOOLS.PACK_DATAPUMP.SP_EXPORT_TABLE (p_directory => 'DATA_PUMP_DIR'
                                       , p_file_name => 'ARQUIVO.DMP'
                                       , p_tables    => 'HR.JOBS,HR.DEPARTMENTS'
                                       , p_db_link   => NULL);
END;
/
```
- No exemplo acima, estamos utilizando a procedure SP_EXPORT_TABLE onde passamos como parâmetros do diretório onde será salvo o arquivo .DMP e .LOG.
- A partir do p_file_name geramos o arquivo .DMP e um arquivo de log com o sufixo _LOG e a extesão .LOG.
- No parâmetro p_tables passamos todas as tabelas que desejamos exportar, estas podem ser ou não do mesmo esquema.

### Export de esquema inteiro
```sh
BEGIN
    TOOLS.PACK_DATAPUMP.SP_EXPORT_SCHEMA (p_directory => 'DATA_PUMP_DIR'
                                        , p_file_name => 'ARQUIVO.DMP'
                                        , p_schema    => 'HR'
                                        , p_db_link   => NULL)
END;
/
```
Utilizando a procedure SP_EXPORT_SCHEMA podemos realizar o export do esquema inteiro.

### Import de arquivo
```sh
BEGIN
    TOOLS.PACK_DATAPUMP.SP_IMPORT_DUMP (p_directory => 'DATA_PUMP_DIR'
                                      , p_file_name => 'ARQUIVO.DMP'
                                      , p_parameters => 'SKIP');
END;
/
```
 Para executarmos o import, é mais simples. só temos de inidicar o diretório a ser utilizado e o nome do arquivo que será importado. Os parâmetros aceitos pelo p_parameters são os mesmos referentes ao TABLE_EXISTS_ACTION (SKIP, TRUNCATE, REPLACE, APPEND). Caso o parâmetro não seja passado, o default 'SKIP.

### Export de arquivo com TTS
 ```sh
 BEGIN
     TOOLS.PACK_DATAPUMP.SP_EXPORT_TRANSPORTABLE (p_directory      => 'DATA_PUMP_DIR'
                                                , p_tablespace     => 'TBS_EXEMPLO'
                                                , p_file_name      => 'ARQUIVO.DMP');
END;
/
 ```
 Com a SP_IMPORT_TRANSPORTABLE podemos realizar o export de TTS utilizando um script .sql através do Flyway. Porém o desenvolvedor deve realizar o processor de alterar os estados da tablespaces. Caso exista alguma dúvida de como realizar todo o processo do Transport Tablespace, favor ler a seguinte documentação: [Transport Tablespace](https://docs.google.com/a/geofusion.com.br/document/d/1feUYN0oLlVMRqIeb4ZD5w0KXmYdBdq2DnNgSOtEk1qM/edit?usp=sharing).

### Import de arquivo com TTS
 ```sh
 BEGIN
     TOOLS.PACK_DATAPUMP.SP_IMPORT_TRANSPORTABLE (p_directory      => 'DATA_PUMP_DIR'
                                                , p_tablespace     => 'TBS_EXEMPLO'
                                                , p_file_name      => 'ARQUIVO.DMP'
                                                , p_data_file_path => '/oracle/oradata/gfn/data_file_example.dbf');
END;
/
 ```
 Com a SP_IMPORT_TRANSPORTABLE podemos realizar o import de TTS utilizando um script .sql através do Flyway.
