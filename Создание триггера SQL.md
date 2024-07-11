#SQL 

```SQL
USE [ConDir]
GO
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TRIGGER [dbo].[ORG_PeoplesInBranch_UpdateLastUpdateTime]
ON [dbo].[Organization_PeoplesInBranch]
AFTER INSERT, DELETE, UPDATE
AS
BEGIN
    SET NOCOUNT ON;
	
    -- Начало транзакции
    BEGIN TRANSACTION;
    -- Проверка существования строки с Value = 'LastUpdateTime'
    IF EXISTS (SELECT 1 FROM [dbo].[Settings] WHERE [Parameter] = 'LastUpdateId')
    BEGIN
        -- Обновление существующей строки
        UPDATE [dbo].[Settings]
        SET [Value] = NEWID()
        WHERE [Parameter] = 'LastUpdateId';
    END
    ELSE
    BEGIN
        -- Вставка новой строки, если она не существует
        INSERT INTO [dbo].[Settings] ([Parameter], [Value])
        VALUES ('LastUpdateId', NEWID());
    END
    -- Завершение транзакции
    COMMIT TRANSACTION;
END;
```
