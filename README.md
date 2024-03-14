SELECT
    'CREATE TABLE ' || table_schema || '.' || table_name || ' (' AS create_statement,
    string_agg(column_name || ' ' || data_type || 
        CASE 
            WHEN character_maximum_length IS NOT NULL THEN '(' || character_maximum_length || ')'
            ELSE ''
        END
        || 
        CASE 
            WHEN is_nullable = 'NO' THEN ' NOT NULL'
            ELSE ''
        END, ', ' ORDER BY ordinal_position) ||
    CASE 
        WHEN constraint_type = 'PRIMARY KEY' THEN ', PRIMARY KEY (' || string_agg(column_name, ', ') || ')'
        ELSE ''
    END
    || ');' AS create_table_statement
FROM
    information_schema.columns
    LEFT JOIN information_schema.key_column_usage USING (table_schema, table_name, column_name)
    LEFT JOIN information_schema.table_constraints USING (constraint_schema, constraint_name)
WHERE
    table_name = 'your_table_name'
GROUP BY
    table_schema,
    table_name,
    constraint_type;
