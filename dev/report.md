Bonjour prof,
J’ai  réfléchi avec le tokenizer et J’ai parcouru les librairies svg. Il en ressort ce qui suit.
En fait je pense que l’un des buts en cherchant à construire l’ast de tout le code c'est d’insérer les fonctions intrinsèques dans le flot d’un programme c et de le visualiser dans le contexte d’exécution de ce programme. C’est un avantage indéniable. On pourrait visualiser le code par l’ast et à tout moment (en cliquant par exemple) décider de visualiser la fonction intrinsèque particulièrement.
Mais l'outil le plus haut niveau disponible actuellement dans ce sens est un tokenizer. Le problème c’est que le tokenizer génère  juste une liste de tokens. Donc  la construction de l'ast nécessite soit d'étendre un parseur existant (comme cparse-master) afin qu'il reconnaisse les types et les fonctions intrinsèques, soit de construire un tel parseur en s’appuyant sur le tokenizer.  J’ai essayé cette dernière démarche, mais je n’ai pas beaucoup avancé. Aussi, je propose que nous laissions cet aspect de côté pour l'instant et nous concentrons sur la modélisation et la visualisation des fonctions intrinsèques en dehors de tout programme quelconque. Pour ce qui est des librairies svg, je pense qu’au cours de l’étude des librairies que j’ai faites, et je pense que ce sont les principales, j’ai raffiné le besoin principal que nous  pouvions attendre de ces librairies.
En effet, la principale utilité d’une telle librairie compte tenu de ce que nous voulons faire, c’est de manipuler les objets du DOM par leur simple invocation. C'est-à-dire une librairie qui nous permette de relier deux éléments html (qui peuvent être des figures) par une droite en passant leur identifiant (id, class ou autre) en paramètre et pas leur coordonnée (ce qui nous obligerait à les déterminer nous mêmes). Des huit librairies que j’ai étudiées, à savoir :
* svg.js,
* two.js,
* jQuery (plus précisément un de ses plugin) ;
* D3.js,
* snap.svg, 
* Treant.js,
* raphael.js,
* jsplumb,

Il n’y a que deux qui adoptent véritablement cette approche, à savoir jsplumb et treant.js.
Jsplumb est payant (même s’il y a une version gratuite avec des fonctionnalités trop limitée). Treant est open source, avec des dépendances jQuery et Raphael. Mais le principal problème de treant.js, hors mis les dépendances,  c’est qu’il est encore en cours de développement. 
Cette situation semble bizarre, comme l’a souligné Stephan Rauh sur sa  page ( last updadte 27 juillet 2017) :
[https://www.beyondjava.net/how-to-connect-html-elements-with-an-arrow-using-svg)](https://www.beyondjava.net/how-to-connect-html-elements-with-an-arrow-using-svg)


Donc je propose ce matin (et les jours suivants) de travailler à classifier et visualiser d’abord les fonctions suivantes :

### exemple 1


* _mm512_add_epi64
* _mm512_srli_epi64
* _mm512_mullo_epi64
* _mm512_rorv_epi32
* _mm512_cvtepi64_epi32
* _mm512_rorv_epi32


### exemple 2

* _mm_slli_si128
* _mm_add_epi32



### exemple 3

* _mm256_set_epi8
* _mm256_shuffle_epi8
* _mm256_and_si256
* _mm256_srli_epi16
* _mm256_slli_epi16
* _mm256_or_si256


### exemple 4

* _mm_min_epu16
* _mm_max_epu16
* _mm_alignr_epi8

et sur cette base de terminer l’article. Tous les autres aspects (ast, raffinement de la modélisation et de la classification des fonctions intrinsèques seraient renvoyés en dehors du champ de porté de l’article).

Donc en résumé, je propose de travailler directement à la classification et la visualisation de ces exemples (qui incluent la génération ou la saisie des données éventuelles, l'affinement des graphiques, etc) et la finalisation de l'article sur cette hypothèse.

pour le code sur les arbres:
Je disais que pour l'affichage, je peux calculer les  paramètres globaux pour le rendu que je veux, l'une des choses les plus facile aussi étant de modifier les variables de translation (ou de rotation) localement en fonction de la profondeur.

    function pictureRighttree(ast,g)
    {
  
    var gr=document.createElementNS(ns, "g");
       //I can put an if or case here on the depth of the tree, and move to xt, yt and angle fixed values to improve the display of the tree (in fact avoid stacking).
    var xt=depth(ast)*coefCox*x0t+x0t, yt=reverseDepth(ast)*coefCoy*y0t+y0t,ang=depth(ast)*coefAng*angt+angt; 
    var cr=drawnSvgCircle(0, 0, radius, ast[2][0], gr)[0], tr=drawnSvgCircle(0, 0, radius, ast[2][0], gr)[1];
    var lr=drawnSvgLine(0, 0, -xt, -yt, gr), yto=-yt, ango=-ang;
    gr.setAttribute("transform", "translate("+xt+","+yt+") rotate("+ango+", 0, "+yto+")");
    gr.appendChild(cr);
    gr.appendChild(tr);
    gr.appendChild(lr);
    g.appendChild(gr);
    return gr;
  }
