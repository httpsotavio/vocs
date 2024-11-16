


# Changes to do
faça as alterações descritas em cada tópico para que o sistema possa funcionar sem problemas

observação:
é recomendado que toda vocação tenha o mesmo healthgain e managain, definidos em vocation.xml

## DATABASE
execute o seguinte comando no seu banco de dados:

```sql
CREATE TABLE player_vocations (
    player_id INT NOT NULL,
    vocation_id INT NOT NULL,
    level INT NOT NULL DEFAULT 1,
    experience INT NOT NULL DEFAULT 0,
    PRIMARY KEY (player_id, vocation_id),
    FOREIGN KEY (player_id) REFERENCES players(id)
) ENGINE=InnoDB;
```

## LUA
navegue até data/lib/compat e abra o arquivo compat.lua, insira as seguintes funções:
```lua
function Player.getVocation(self)
	return self:getCurrentVocation()
end

function Player.setVocation(self, param)
	return self:changeVocation(param)
end
```
