


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
    experience BIGINT NOT NULL DEFAULT 0,
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

em seu arquivo de login.lua, adicione o seguinte código, antes do último return true:
```lua
if (not player:hasVocation(player:getVocation():getId())) then
	player:addVocation(player:getVocation():getId(), player:getLevel(), player:getExperience())
	local outfit = player:getOutfit()
	outfit.lookType = player:getVocation():getOutfit()
	player:setOutfit(outfit)
end
```

## VOCATIONS.XML CHANGES
em seu vocations.xml, adicione a tag "outfit", ficando algo como "outfit = 10", isso fará com que o personagem mude de outfit automaticamente quando o player mudar de vocação

a outfit passada como parâmetro deve estar definida em outfits.xml, sendo passado o lookType da outfit como parametro dentro de vocations.xml


caso nenhuma "outfit" seja passada a uma vocação em vocations.xml, por padrão, a outfit do player será trocado para a outfit com looktype 10, se esta existir

### NEW FUNCTIONS

```lua
player:getCurrentVocation() // retorna a atual vocação do jogador
player:getVocationList() // retorna uma tabela com todas as vocações liberadas pelo jogador, tendo como índice o id da vocação, e como valor o level da vocação
player:addVocation(vocationId, level[, experience[) // adiciona uma vocação ao jogador, tendo o level como padrão 1, e o experience como padrão 0 caso estes parâmetros não sejam passados
player:removeVocation(vocationId) // remove a vocação do jogador
player:changeVocation(vocationId) // altera a vocação atual do jogador para a vocação passada como parâmetro, esta tendo que conter na lista de vocações do jogador
player:hasVocation(vocationId) // veriica se o jogador sem a vocação passada em sua lista de vocações
```