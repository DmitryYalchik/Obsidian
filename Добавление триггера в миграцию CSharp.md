#CSharp #SQL 

```CSharp
public partial class AddUpdateLastUpdateTimeTrigger : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.Sql(@"
            CREATE TRIGGER Table_UpdateLastUpdateId
			   ON  Table
			   AFTER INSERT,DELETE,UPDATE
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
			END
        ");
    }
    protected override void Down(MigrationBuilder migrationBuilder)
    {
        
    }
}```
