实验一：依赖分析和依赖图
------------------------

-  滕佳倩 201931990407
-  吴支飞 201931990232
-  温启涛 201931990228
-  杨佳宁 201931990414
-  王景瑶 201931990508

实验目的
~~~~~~~~

-  根据出软件应用程序的源代码，找出模块/类/函数之间的依赖关系
-  分析EnglishPal（新版本）的依赖
-  分析EnglishPal（旧版本）的依赖
-  对比两个版本的EnglishPal并判断哪个更好
-  分析课本中第四章要求中类的依赖关系

介绍
~~~~

本次实验针对EnglishPal新旧版本以及课本中给出的样例的源代码，分析当前模块/类/函数之间的依赖关系，分析当前体系结构。

材料和方法
~~~~~~~~~~

1. 团队分工

   -  滕佳倩：分析表格，从5个方面对EnglishPal新旧版本进行对比
   -  吴支飞：根据函数、类之间的关系，生成mermaid图
   -  温启涛：根据各模块之间的关系，生成dot文件
   -  杨佳宁：回答问题，对EnglishPal新旧版本进行打分
   -  王景瑶：编写实验报告

2. 开发语言及工具

   -  Windows 10操作系统，Python语言开发环境
   -  Snakefood：从Python代码中生成依赖，过滤，聚类，并从依赖列表中生成图表。使用Snakefood捕获模块级依赖关系。
   -  Graphviz：开源的图形可视化软件。使用graphviz
      online渲染依赖关系图。
   -  Mermaid：基于Javascript的图表和图表工具，使用文本和代码创建图表和可视化，以便动态地创建和修改图表。使用mermaid
      live editor渲染类/函数级依赖关系图。

结果
~~~~

-  snakefood-beginningofspring.dot
   .. code:: graphviz
       strict digraph "dependencies" {
        graph [
            rankdir = "LR",
            overlap = "scale",
            size = "10,10",
            ratio = "fill",
            fontsize = "16",
            clusterrank = "local"
            ]
            node [
            fontsize=12
            shape=box
        ];
        "account_service.py" -> "Login.py"
        "Article.py" -> "WordFreq.py"
        "Article.py" -> "wordfreqCMD.py"
        "Article.py" -> "UseSqlite.py"
        "Article.py" -> "pickle_idea.py"
        "Article.py" -> "pickle_idea2.py"
        "Article.py" -> "difficulty.py"
        "difficulty.py" -> "wordfreqCMD.py"
        "Login.py" -> "UseSqlite.py"
        "main.py" -> "Login.py"
        "main.py" -> "Article.py"
        "main.py" -> "Yaml.py"
        "main.py" -> "user_service.py"
        "main.py" -> "account_service.py"
        "user_service.py" -> "Yaml.py"
        "user_service.py" -> "Article.py"
        "user_service.py" -> "WordFreq.py"
        "user_service.py" -> "wordfreqCMD.py"
        "user_service.py" -> "pickle_idea.py"
        "user_service.py" -> "pickle_idea2.py"
        "WordFreq.py" -> "wordfreqCMD.py"
        "wordfreqCMD.py" -> "pickle_idea.py"
    }

-  snakefood-chapter4.dot
   .. code:: graphviz
        strict digraph "dependencies" {
            graph [
                rankdir = "LR",
                overlap = "scale",
                size = "10,10",
                ratio = "fill",
                fontsize = "16",
                clusterrank = "local"
                ]
                node [
                fontsize=12
                shape=box
            ];
            "flask_app.py" -> "model.py"
            "flask_app.py" -> "orm.py"
            "flask_app.py" -> "repository.py"
            "flask_app.py" -> "services.py"
            "services.py" -> "model.py"
            "services.py" -> "repository.py"
            "orm.py" -> "model.py"
            "repository.py" -> "model.py"
        }

-  snakefood-colddew.dot
   .. code:: graphviz
        strict digraph "dependencies" {
            graph [
                rankdir = "LR",
                overlap = "scale",
                size = "10,10",
                ratio = "fill",
                fontsize = "16",
                clusterrank = "local"
                ]
                node [
                    fontsize=12
                    shape=box
                ];
            "difficulty.py" -> "wordfreqCMD.py"
            "main.py" -> "WordFreq.py"
            "main.py" -> "wordfreqCMD.py"
            "main.py" -> "UseSqlite.py"
            "main.py" -> "pickle_idea.py"
            "main.py" -> "pickle_idea2.py"
            "main.py" -> "difficulty.py"
            "WordFreq.py" -> "wordfreqCMD.py"
            "wordfreqCMD.py" -> "pickle_idea.py"
        }

-  mermaid-beginningofspring.txt

   .. code:: mermaid

      classDiagram
          difficulty ..> wordfreqCMD
          difficulty ..> pickle
          main ..> Login
          main ..> Article
          main ..> Yaml
          main ..> user_service
          main ..> account_service
          pickle_idea ..> pickle
          pickle_idea2 ..> pickle
          Article ..> wordfreqCMD
          Article ..> WordFreq
          Article ..> pickle_idea
          Article ..> pickle_idea2
          Article ..> difficulty
          Article ..> InsertQuery
          Article ..> RecordQuery
          user_service ..> Yaml
          user_service ..> Article
          user_service ..> WordFreq
          user_service ..> wordfreqCMD
          user_service ..> pickle_idea
          user_service ..> pickle_idea2
          account_service ..> Login
        Login ..> InsertQuery
        Login ..> RecordQuery
          WordFreq ..> wordfreqCMD
          wordfreqCMD ..> pickle_idea
        InsertQuery --|> Sqlite3Template
        RecordQuery --|> Sqlite3Template

      class Article{
          +total_number_of_essays()
          +get_article_title(s)
          +get_article_body(s)
          +get_today_article(user_word_list, articleID)
          +load_freq_history(path)
          +within_range(x, y, r)
          +get_question_part(s)
          +get_answer_part(s)
      }

      class difficulty{
          +load_record(pickle_fname)
          +difficulty_level_from_frequency(word, d)
          +get_difficulty_level(d1, d2)
          +revert_dict(d)
          +user_difficulty_level(d_user, d)
          +text_difficulty_level(s, d)
      }

      class Login{
          +verify_user(username, password)
          +add_user(username, password)
          +check_username_availability(username)
          +change_password(username, old_password, new_password)
          +md5(s)
      }

      class main{
          +get_random_image(path)
          +get_random_ads()
          +appears_in_test(word, d)
          +mark_word()
          +mainpage()
      }

      class pickle_idea{
          +lst2dict(lst, d)
          + dict2lst(d) 
          +load_record(pickle_fname)
          +save_frequency_to_pickle(d, pickle_fname)
          +unfamiliar(path,word)
          +familiar(path,word)
      }

      class pickle_idea2{
          +lst2dict(lst, d)
          +deleteRecord(path,word)
          +dict2lst(d)
          +merge_frequency(lst1, lst2)
          +load_record(pickle_fname)
          +save_frequency_to_pickle(d, pickle_fname)
      }

      class user_service{
          +user_reset(username)
          +unfamiliar(username, word)
          +familiar(username, word)
          +deleteword(username, word)
          +userpage(username)
          +user_mark_word(username)
          +get_time()
          +get_flashed_messages_if_any()
      }

      class account_serive{
          +signup()
          +login()
          +logout()
          +reset()
      }

      class Sqlite3Template{
          +__init__(self, db_fname)
          +connect(self, db_fname)
          +instructions(self, query_statement)
          +operate(self)
          +format_results(self)
          +do(self)
          +instructions_with_parameters(self, query_statement, parameters)
          +do_with_parameters(self)
          +operate_with_parameters(self)
      }

      class InsertQuery{
          +instructions(self, query)
      }

      class RecordQuery{
          +instructions(self, query)
          +format_results(self)
          +get_results(self)
      }

      class WordFreq{
          +__init__(self, s)
          +get_freq(self)
      }

      class wordfreqCMD{
          +freq(fruit)
          +youdao_link(s)
          +file2str(fname)
          +remove_punctuation(s)
          +sort_in_descending_order(lst)
          +sort_in_ascending_order(lst)
          +make_html_page(lst, fname)
      }

      class Yaml{
          
      }

-  mermaid-chapter4.txt

-  mermaid-colddew.txt

   .. code:: mermaid

      classDiagram
          difficulty ..> wordfreqCMD
          main ..> wordfreqCMD
          main ..> WordFreq
          main ..> InsertQuery
          main ..> RecordQuery
          pickle_idea ..> pickle
          pickle_idea2 ..> pickle
          WordFreq ..> wordfreqCMD
          wordfreqCMD ..> pickle_idea
        InsertQuery --|> Sqlite3Template
        RecordQuery --|> Sqlite3Template

      class difficulty{
          +load_record(pickle_fname)
          +difficulty_level_from_frequency(word, d)
          +get_difficulty_level(d1, d2)
          +revert_dict(d)
          +user_difficulty_level(d_user, d)
          +text_difficulty_level(s, d)
      }

      class main{
          +get_random_image(path)
          +get_random_ads()
          + total_number_of_essays()
          +load_freq_history(path)
          +verify_user(username, password)
          +add_user(username, password)
          +check_username_availability(username)
          +get_expiry_date(username)
          +within_range(x, y, r)
          +get_article_title(s)
          +get_article_body(s)
          +get_today_article(user_word_list, articleID)
          +appears_in_test(word, d)
          +get_time()
          +get_question_part(s)
          +get_answer_part(s)
          +get_flashed_messages_if_any()
          +user_reset(username)
          +mark_word()
          +mainpage()
          +user_mark_word(username)
          +unfamiliar(username,word)
          +familiar(username,word)
          +deleteword(username,word)
          +userpage(username)
          +signup()
          +login()
          +logout()
      }

      class pickle_idea{
          +lst2dict(lst, d)
          + dict2lst(d) 
          +merge_frequency(lst1, lst2)
          +load_record(pickle_fname)
          +save_frequency_to_pickle(d, pickle_fname)
          +unfamiliar(path,word)
          +familiar(path,word)
      }

      class pickle_idea2{
          +lst2dict(lst, d)
          + deleteRecord(path,word)
          +dict2lst(d)
          +merge_frequency(lst1, lst2)
          +load_record(pickle_fname)
          +save_frequency_to_pickle(d, pickle_fname)
      }

      class Sqlite3Template{
          +__init__(self, db_fname)
          +connect(self, db_fname)
          +instructions(self, query_statement)
          +operate(self)
          +format_results(self)
          +do(self)
          +instructions_with_parameters(self, query_statement, parameters)
          +do_with_parameters(self)
          +operate_with_parameters(self)
      }

      class InsertQuery{
          +instructions(self, query)
      }

      class RecordQuery{
          +instructions(self, query)
          +format_results(self)
          +get_results(self)
      }

      class WordFreq{
          +__init__(self, s)
          +get_freq(self)
      }

      class wordfreqCMD{
          +freq(fruit)
          +youdao_link(s)
          +file2str(fname)
          +remove_punctuation(s)
          +sort_in_descending_order(lst)
          +sort_in_ascending_order(lst)
          +make_html_page(lst, fname)
      }

讨论分析
~~~~~~~~

1. 分析对比 BeginningOfSpring 和 ColdDew
2. 问题分析:*Fill out Table 1. From a scale 1 (worst) to scale 5 (best),
   how would you evaluate the architectural health of each version of
   EnglishPal? Which version of EnglishPal is easier to understand and
   maintain? Explain in no more than 3 sentences.*

   -  beginningofspring：4分、colddew：3分
   -  beginningofspring的版本更加易于理解和维护
   -  原因：

      1. codedew版本的很多函数都写在main里面，多个功能杂糅在一起，耦合性过高
      2. beginningofspring把很多功能都分开了，互相提供接口，如果需要改动的话直接改动一小部分就好了，符合“高内聚，低耦合”的原则

参考文献
~~~~~~~~

[1] Martin Blais, Snakefood User Manual, Copyright (C) 2001-2007
