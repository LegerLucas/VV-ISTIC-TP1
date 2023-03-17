# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Il s’est passé quelque chose d’étrange sur la page Youtube de Gangnam  Style. En effet, le compteur de vues resté un temps bloqué à 2 147 483  647 vues. Ce bug local qui a pour origine un simple fait : **Google n’avait jamais prévu qu’une vidéo ferait autant de vues.**

   En effet, le compteur **a commencé à afficher n’importer quoi,** passant de chiffres négatifs à des chiffres complètements farfelus. Un bug dû à l’entier 32 bits qu’utilise Google pour comptabiliser ses vues, qui ne va donc pas au delà des 2 147 483 647 vues. Pour résoudre le problème, ils ont passer le nombre de vue youtube d'un entier 32 bit à un entier 64 bit.

2. Bug apache : pollFirst() et pollLast() sont exécutés avec succès et ne lèvent pas une `UnsupportedOperationException` sur une instance de `UnmodifiableNavigableSet`.

   A mon avis, `org.apache.commons.collections4.set.UnmodifiableNavigableSet` devrait avoir une implémentation similaire à `java.util.Collections.UnmodifiableNavigableSet`, où les deux méthodes lèvent une `UnsupportedOperationException` : https://github.com/openjdk/jdk/blob/708407eddc9d52c01de02e3986c05e1c6225fa85/src/java.base/share/classes/java/util/Collections.java#L1278-L1279.

   Ce problème a été détecté lors du travail sur https://github.com/assertj/assertj-core/pull/2328 et résolu dans https://github.com/apache/commons-collections/pull/250 remplacement de try catch par des assertThrown

3. Sur la base de ces principes, définissez une expérience :

   1. Commencez par définir l'état d'équilibre comme une sortie mesurable d'un système qui indique un
      un comportement normal.
   2. Hypothèse : cet état d'équilibre se maintiendra dans le groupe témoin et dans le groupe
      groupe expérimental.
   3. Introduisez des variables qui reflètent des événements réels tels que des serveurs qui tombent en panne, des disques durs qui fonctionnent mal, des connexions réseau qui sont coupées.
   4. Essayez de réfuter l'hypothèse en recherchant une digroupe de contrôle et le groupe expérimental.fférence dans l'état d'équilibre entre le groupe témoin et le groupe expérimental.

4. La spécification formelle pour WebAssembly présente des avantages significatifs pour l'interopérabilité, la clarté et la sécurité de cette technologie. Cependant, les tests sont tout aussi importants pour garantir que les implémentations respectent la spécification et fonctionnent de manière fiable dans différents environnements. Les implémentations réelles de WebAssembly sont souvent sujettes à des bugs et des erreurs d'où l'importance des tests

5. la spécification mécanisée peut être utilisée pour générer  automatiquement des tests et des outils de validation pour les  implémentations de WebAssembly, ce qui facilite la vérification de la  conformité des implémentations à la spécification. Enfin, la  spécification mécanisée peut être utilisée pour explorer les propriétés  et les limites de WebAssembly, ce qui peut aider à identifier les  améliorations et les extensions potentielles du langage.

   L'auteur note également que la spécification mécanisée a permis  d'améliorer la spécification formelle originale du langage. En  particulier, elle a aidé à identifier et à corriger certaines erreurs et ambiguïtés dans la spécification originale.

   L'auteur a également dérivé d'autres artefacts de la spécification mécanisée, notamment une machine virtuelle formelle pour WebAssembly,  qui peut être utilisée pour simuler l'exécution de programmes  WebAssembly et vérifier leur conformité à la spécification. Il a  également généré automatiquement des tests pour la spécification, ainsi  que des outils de validation pour les implémentations de WebAssembly.

   L'auteur a vérifié la spécification en utilisant un assistant de preuve interactif appelé Isabelle/HOL. Il a formalisé la spécification de WebAssembly en utilisant une sémantique opérationnelle, qui définit le comportement de l'exécution des programmes WebAssembly. Il a ensuite utilisé Isabelle/HOL pour prouver mathématiquement que la spécification était correcte et cohérente.

   Bien que la spécification mécanisée pour WebAssembly réduise le risque d'erreurs dans les implémentations, elle ne supprime pas le besoin de tests. Les tests restent essentiels pour détecter les erreurs et les bugs dans les implémentations et pour vérifier la conformité des implémentations à la spécification. Cependant, la spécification mécanisée peut faciliter la génération de tests automatiques et la validation des implémentations.