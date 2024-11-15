


new table:
CREATE TABLE player_vocations (
    player_id INT NOT NULL,
    vocation_id INT NOT NULL,
    level INT NOT NULL DEFAULT 1,
    experience INT NOT NULL DEFAULT 0,
    PRIMARY KEY (player_id, vocation_id),
    FOREIGN KEY (player_id) REFERENCES players(id)
) ENGINE=InnoDB;

compat.lua:
function Player.getVocation(self)
	return self:getCurrentVocation()
end