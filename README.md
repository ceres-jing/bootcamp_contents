SELECT 
    'CREATE TABLE ' || table_name || ' (' AS create_statement,
    string_agg(column_name || ' ' || data_type || 
        CASE 
            WHEN character_maximum_length IS NOT NULL THEN '(' || character_maximum_length || ')'
            ELSE ''
        END
        || 
        CASE 
            WHEN is_nullable = 'NO' THEN ' NOT NULL'
            ELSE ''
        END, ', ' ORDER BY ordinal_position) || ');' AS create_table_statement
FROM 
    information_schema.columns
WHERE 
    table_name = 'your_table_name'
GROUP BY 
    table_name;
