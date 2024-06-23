Functions
Atividade de banco de dados MySQL(Functions)

Este trabalho simula um banco de dados de um setor escolar, onde há a necessidade de cadastrar alunos, suas matérias, seus cursos e os emails escolares Além de apenas armazenar, esse trabalho também automatiza certas tarefas como matriculas e criação de emails institucionais com o uso de functions e procedures

Na primeira parter do programa é simples e acontece apenas as criações das tabelas que serão ultilizadas para armazenar os dados:

create database Universidade;

use Universidade;

create table Area (

id		int auto_increment primary key,
nome	varchar(60)
);

create table cursos (

id		int auto_increment primary key,
nome	varchar(60),
id_area	int,

FOREIGN KEY (id_area) REFERENCES Area(id)
);

create table alunos (

RA	int auto_increment primary key,
nome		varchar(60),
sobrenome	varchar(60),
curso_id	int,

FOREIGN KEY (curso_id) REFERENCES cursos(id)
);

create table EmailsAlunos(

id			int	primary key auto_increment,
email		varchar(255),
RA_aluno	int,

FOREIGN KEY (RA_aluno) REFERENCES alunos(RA)
);

Com as tabelas criadas partimos para as partes automatizadas do programa, as functions e procedures

Essa primeira automação é uma procedure que cria o email de um aluno automaticamente e também ja o adiciona na tabela de emails institucionais:

/----------------------------Procedure para Criar Email--------------------------------------/

DELIMITER //

CREATE PROCEDURE EmailAlunos(IN nomeprocedure VARCHAR(100),IN sobreprocedure varchar(100),IN uniprocedure varchar(100),IN RaProcedure int) BEGIN

DECLARE email1	 VARCHAR(255);
DECLARE nome1 	 VARCHAR(255);
DECLARE nome2	 VARCHAR(255);
DECLARE nomeuni  varchar(255);
DECLARE RaEmail	 int;


SET nome1 = nomeprocedure;
SET nome2 = sobreprocedure;
SET nomeuni = uniprocedure;
SET RaEmail = RaProcedure;

SET email1 = CONCAT(nome1, '.', nome2, '@', nomeuni, '.com');

INSERT INTO EmailsAlunos(email, RaAluno) VALUES (email1,RaEmail);
SELECT * FROM EmailsAlunos;
END; //

DELIMITER ;

call criar_email_aluno('João','silva','FACENS',1); select * from EmailsAlunos;

A próxima automação mostra o id de um curso ja inserido no banco de dados baseado no nome, ou seja, voce digita o nome do curso e aparece seu id:

/----------------------------Rotina para identificar o Id---------------------------------/

DELIMITER //

CREATE FUNCTION Idcurso(nomecurso varchar(60), nomearea varchar(60)) RETURNS int DETERMINISTIC BEGIN

DECLARE IdCurso;

select id from Cursos where nome = nomecurso;

RETURN idCurso = id;
END;

DELIMITER //

Esta outra automação faz com que seja mais facil adicionar cursos, sem precisar digitar os códigos de Insert Into completamente, apenas chamar a procedure:

DELIMITER //

CREATE PROCEDURE AddCurso(IN NomeCursos varchar(60),IN IdArea int) BEGIN

DECLARE NomeCursosc	 varchar(60);
DECLARE IdAreac	 int;



SET NomeCursosc = NomeCursos;
SET IdAreac = IdArea;

INSERT INTO cursos(nome,id_area) VALUES (NomeCursosc, IdAreac);
SELECT nome FROM Cursos where id = IdAreac;
END; //

DELIMITER ;

Esta próxima procedure automatiza a inserção do aluno no banco de dados(Realiza as matrículas):

/----------------------------Automação de Matrícula---------------------------------/ DELIMITER //

CREATE PROCEDURE Matricula(IN NomeAluno varchar(60),IN SobrenomeAluno varchar(60), CursoAluno int, Uni varchar(60)) BEGIN

DECLARE nomealuno	 	varchar(60);
DECLARE sobrenomealuno	varchar(60);
DECLARE cursoaluno	 	int;
DECLARE uni varchar(60);

SET nomealuno = NomeAluno;
SET sobrenomealuno = SobrenomeAluno;
SET cursoaluno = CursoAluno;
set uni = Uni;

SELECT COUNT(*) INTO nome,sobrenome FROM Pessoas WHERE nome = nomealuno and sobrenome = sobrenome;
INSERT INTO alunos(nome,sobrenome,curso_id) VALUES (nomealuno,sobrenomealuno,cursoaluno);
call EmailAluno(nomealuno,sobrenomealuno,uni);
END; // DELIMITER ;

Após todas as automações e criações de tabelas falta apenas inserir os dados, os códigos a seguir apenas inserem alunos, areas e cursos em suas tabelas, lembrando que a tabela de emails não tem inserções diretas pois é chamada através da procedure automatizada:

/----------------------------Inserções de Alunos---------------------------------/

INSERT INTO alunos (nome, sobrenome, curso_id) VALUES

('Carlos', 'Nogueira', 1),

('Sabrina', 'Vieira', 2),

('Ana', 'Silva', 3),

('Bruno', 'Souza', 4),

('Carla', 'Oliveira', 5),

('Daniel', 'Pereira', 6),

('Eduarda', 'Costa', 7),

('Felipe', 'Rodrigues', 8),

('Gabriela', 'Martins', 9),

('Henrique', 'Lima', 10),

('Isabela', 'Ferreira', 11),

('João', 'Almeida', 12),

('Karina', 'Gomes', 13),

('Leonardo', 'Ribeiro', 14),

('Mariana', 'Mendes', 15),

('Nicolas', 'Barbosa', 16),

('Olivia', 'Santos', 17),

('Pedro', 'Cardoso', 18),

('Quintino', 'Teixeira', 19),

('Renata', 'Leite', 20),

('Sandro', 'Nunes', 21),

('Tatiana', 'Azevedo', 22),

('Ulisses', 'Vieira', 23),

('Valéria', 'Campos', 24),

('Wagner', 'Rocha', 25),

('Xavier', 'Monteiro', 26),

('Yasmin', 'Dias', 27),

('Zeca', 'Rezende', 28),

('Amanda', 'Carvalho', 1),

('Bernardo', 'Pinto', 2),

('Camila', 'Andrade', 3),

('Diego', 'Ramos', 4),

('Elisa', 'Freitas', 5),

('Fernando', 'Cruz', 6),

('Giovana', 'Moraes', 7),

('Hugo', 'Barros', 8),

('Ingrid', 'Gonçalves', 9),

('Jorge', 'Nascimento', 10),

('Kelly', 'Araújo', 11),

('Lucas', 'Silveira', 12),

('Marcos', 'Moreira', 13),

('Natália', 'Viana', 14),

('Otávio', 'Neves', 15),

('Patrícia', 'Ramos', 16),

('Rafael', 'Siqueira', 17),

('Sara', 'Lourenço', 18),

('Thiago', 'Maia', 19),

('Vitor', 'Farias', 20),

('Beatriz', 'Castro', 21),

('Carlos', 'Meireles', 22),

('Diana', 'Tavares', 23),

('Emanuel', 'Vieira', 24),

('Fábio', 'Pereira', 25),

('Gustavo', 'Mendes', 26),

('Helena', 'Carvalho', 27),

('Igor', 'Silva', 28),

('Juliana', 'Cunha', 1),

('Larissa', 'Costa', 2),

('Marcelo', 'Oliveira', 3),

('Nicole', 'Melo', 4),

('Paulo', 'Cruz', 5),

('Rita', 'Barbosa', 6),

('Sofia', 'Fernandes', 7),

('Vinícius', 'Moraes', 8),

('Adriana', 'Santos', 9),

('Breno', 'Dias', 10),

('Clarissa', 'Matos', 11),

('Daniela', 'Lima', 12),

('Eduardo', 'Carvalho', 13),

('Fernanda', 'Sousa', 14),

('Guilherme', 'Teixeira', 15),

('Isabel', 'Martins', 16),

('Júlio', 'Silva', 17),

('Leandro', 'Almeida', 18),

('Marta', 'Ramos', 19),

('Nathan', 'Ferreira', 20),

('Otília', 'Moura', 21),

('Pedro', 'Lima', 22),

('Quésia', 'Vieira', 23),

('Ricardo', 'Pires', 24),

('Sônia', 'Barros', 25),

('Túlio', 'Oliveira', 26),

('Verônica', 'Santos', 27),

('William', 'Gonçalves', 28),

('Ximena', 'Costa', 1),

('Yuri', 'Macedo', 2),

('Zélia', 'Moreira', 3),

('Arthur', 'Souza', 4),

('Bárbara', 'Freitas', 5),

('Caio', 'Oliveira', 6),

('Davi', 'Pereira', 7),

('Estela', 'Cunha', 8),

('Francisco', 'Barbosa', 9),

('Gabriel', 'Dias', 10),

('Heloísa', 'Azevedo', 11),

('Iara', 'Fernandes', 12),

('Joaquim', 'Teixeira', 13),

('Kleber', 'Silva', 14),

('Letícia', 'Oliveira', 15),

('Miguel', 'Mendes', 16),

('Natanael', 'Vieira', 17),

('Oscar', 'Campos', 18),

('Pâmela', 'Ferreira', 19),

('Quirino', 'Gomes', 20),

('Rodrigo', 'Barros', 21),

('Simone', 'Lima', 22),

('Talita', 'Melo', 23),

('Ubirajara', 'Ribeiro', 24),

('Vicente', 'Macedo', 25),

('Wesley', 'Carvalho', 26),

('Xênia', 'Pinto', 27),

('Yago', 'Costa', 28),

('Zoraide', 'Alves', 1),

('Anselmo', 'Martins', 2),

('Bruna', 'Cardoso', 3),

('Cíntia', 'Vieira', 4),

('Douglas', 'Gomes', 5),

('Evelyn', 'Silva', 6),

('Fabrício', 'Oliveira', 7),

('Gilberto', 'Cunha', 8),

('Heloise', 'Barbosa', 9),

('Isis', 'Pereira', 10),

('Jefferson', 'Dias', 11),

('Kelly', 'Mendes', 12),

('Leonora', 'Santos', 13),

('Maurício', 'Teixeira', 14),

('Nádia', 'Vieira', 15),

('Orlando', 'Campos', 16),

('Priscila', 'Fernandes', 17),

('Quirina', 'Gonçalves', 18),

('Rogério', 'Barros', 19),

('Sabrina', 'Lima', 20),

('Tatiane', 'Melo', 21),

('Ursula', 'Ribeiro', 22),

('Valter', 'Macedo', 23),

('Wendel', 'Carvalho', 24),

('Xisto', 'Pinto', 25),

('Yasmin', 'Costa', 26),

('Zenilda', 'Alves', 27),

('Afonso', 'Martins', 28),

('Bianca', 'Cardoso', 1),

('Caetano', 'Vieira', 2),

('Denise', 'Gomes', 3),

('Emílio', 'Silva', 4),

('Fabiana', 'Oliveira', 5),

('Geraldo', 'Cunha', 6),

('Hilda', 'Barbosa', 7),

('Ivone', 'Pereira', 8),

('Jonas', 'Dias', 9),

('Kátia', 'Mendes', 10),

('Luan', 'Santos', 11),

('Márcia', 'Teixeira', 12),

('Nilton', 'Vieira', 13),

('Otília', 'Campos', 14),

('Paulo', 'Fernandes', 15),

('Quirino', 'Gonçalves', 16),

('Rita', 'Barros', 17),

('Sérgio', 'Lima', 18),

('Tereza', 'Melo', 19),

('Ubiratan', 'Ribeiro', 20),

('Vera', 'Macedo', 21),

('Wilson', 'Carvalho', 22),

('Xavier', 'Pinto', 23),

('Yara', 'Costa', 24),

('Zuleica', 'Alves', 25),

('Alex', 'Martins', 26),

('Beatriz', 'Cardoso', 27),

('Caio', 'Vieira', 28),

('Débora', 'Gomes', 1),

('Eliana', 'Silva', 2),

('Fábio', 'Oliveira', 3),

('Gustavo', 'Cunha', 4),

('Helena', 'Barbosa', 5),

('Ivo', 'Pereira', 6),

('Joana', 'Dias', 7),

('Kleber', 'Mendes', 8),

('Luana', 'Santos', 9),

('Márcio', 'Teixeira', 10),

('Nádia', 'Vieira', 11),

('Oscar', 'Campos', 12),

('Patrícia', 'Fernandes', 13),

('Quirina', 'Gonçalves', 14),

('Raimundo', 'Barros', 15),

('Sílvia', 'Lima', 16),

('Tarcísio', 'Melo', 17),

('Úrsula', 'Ribeiro', 18),

('Vagner', 'Macedo', 19),

('Wanderley', 'Carvalho', 20),

('Xenofonte', 'Pinto', 21),

('Yuri', 'Costa', 22),

('Zilda', 'Alves', 23),

('João', 'Matos', 24),

('Vânia', 'Gomes', 25),

('Roberto', 'Silva', 26),

('Cláudia', 'Rocha', 27),

('Rogério', 'Santos', 28),

('Márcia', 'Ribeiro', 1),

('Julio', 'Almeida', 2),

('Alana', 'Castro', 3),

('Anderson', 'Fernandes', 4),

('Monique', 'Dias', 5);

/----------------------------Inserções de Areas---------------------------------/

insert into Area (nome) values

('Exatas'),

('Humanas'),

('Ciencias da Natureza');

/----------------------------Inserções de Cursos---------------------------------/

INSERT INTO cursos (nome, id_area) VALUES ('Administração', 2),

('Engenharia da Pesca', 1),

('Fisica', 3),

('Matemática', 1),

('História', 2),

('Biologia', 3),

('Engenharia Civil', 1),

('Direito', 2),

('Química', 3),

('Engenharia de Computação', 1),

('Sociologia', 2),

('Geologia', 3),

('Engenharia Elétrica', 1),

('Filosofia', 2),

('Astronomia', 3),

('Engenharia Mecânica', 1),

('Psicologia', 2),

('Geografia', 3),

('Engenharia Química', 1),

('Antropologia', 2),

('Ciências Ambientais', 3),

('Engenharia de Produção', 1),

('Comunicação Social', 2),

('Fisioterapia', 3),

('Engenharia de Software', 1),

('Letras', 2),

('Medicina', 3),

('Ciência da Computação', 1),

('Pedagogia', 2),

('Odontologia', 3),

('Engenharia Ambiental', 1),

('Relações Internacionais', 2),

('Farmácia', 3),

('Arquitetura', 1),

('Ciências Sociais', 2),

('Nutrição', 3),

('Engenharia de Materiais', 1),

('Administração Pública', 2),

('Veterinária', 3),

('Engenharia de Telecomunicações', 1),

('Serviço Social', 2),

('Zootecnia', 3),

('Engenharia de Alimentos', 1),

('Turismo', 2),

('Educação Física', 3),

('Engenharia Biomédica', 1),

('Biblioteconomia', 2),

('Ecologia', 3);
