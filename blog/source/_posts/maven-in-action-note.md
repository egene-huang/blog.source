maven解析传递性依赖原则:
1. 距离最近为第一原则  例如: A -> B -> C(1.0) ;  A -> C(2.0) 则这时候只C(2.0)会被加入进来.
2. 声明顺序为第二原则 例如 A -> B -> C(1.0) ; A -> D -> C(2.0) 这时候,如果 B优先于D声明 则只C(1.0)会被加入进来,反之加入C(2.0)依赖

在依赖传递性中出现问题时, 使用<exclusions><exclusion>元素排除不必要的传递依赖, 例如 A -> B -> C  这时候 C.jar不能加入进来,否则会出现冲突,例如运行提示NoSuchMethodException异常  这时候 可以在B依赖节点排除C依赖 ; 如果是因为C版本问题冲突则可以先将C排除然后重新声明C指定版本依赖
使用exculsions可以排除多个依赖, 在该节点下只需要声明groupId和artifactId即可,不需要version节点,因为maven不可能把groupId和artifactId相同版本不同的依赖解析进来.
