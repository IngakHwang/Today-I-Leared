# Project-Kepl 정리 2

## 1. servlet-context.xml

이미지파일, js파일 등등 resource 파일 경로 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
	<resources mapping="/plugins/**" location="/resources/plugins/"/>
	<resources mapping="/dist/**" location="/resources/dist/"/>
	
	
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>

	
	
	
	<context:component-scan base-package="mc.sn.KEPL" />
	
</beans:beans>

```

## 2. 게시판CRUD

### 1. 게시판 Table 생성 (Mysql)

한글 설정을 위한 utf8 설정

```sql

CREATE TABLE tb_article (
  article_no int NOT NULL AUTO_INCREMENT,
  title varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  content text CHARACTER SET utf8 COLLATE utf8_general_ci,
  writer varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  regdate timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  viewcnt int DEFAULT 0 NULL,
  PRIMARY KEY (article_no)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;
```

### 2. ArticleVO.java

테이블의 구조 객체화를 시키기 위한 ArticleVO 생성

경로 : src/main/java 에서 VO패키지 안에 생성

```java
public class ArticleVO { 
    private Integer article_no;
    private String title; 
    private String content; 
    private String writer; 
    private Date regDate; 
    private int viewCnt;

    //Getter, Setter, toString 입력
    
}
```

### 3.ArticleDAO.java (Interface)

경로 : src/main/java 에서 DAO패키지 안에 생성

```java
public interface ArticleDAO {

    void create(ArticleVO articleVO) throws Exception;

    ArticleVO read(Integer article_no) throws Exception;

    void update(ArticleVO articleVO) throws Exception;

    void delete(Integer article_no) throws Exception;

    List<ArticleVO> listAll() throws Exception;
    
}
```

### 4.ArticleDAOImpl.java

ArticleDAO 인터페이스를 구현한 ArticleDAO 생성

```java
@Repository
public class ArticleDAOImpl implements ArticleDAO{

	private static final String NAMESPACE = "mc.sn.KEPL.mappers.article.ArticleMapper";

    private final SqlSession sqlSession;

    @Inject
    public ArticleDAOImpl(SqlSession sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public void create(ArticleVO articleVO) throws Exception {
        sqlSession.insert(NAMESPACE + ".create", articleVO);
    }

    @Override
    public ArticleVO read(Integer article_no) throws Exception {
        return sqlSession.selectOne(NAMESPACE + ".read", article_no);
    }

    @Override
    public void update(ArticleVO articleVO) throws Exception {
        sqlSession.update(NAMESPACE + ".update", articleVO);
    }

    @Override
    public void delete(Integer article_no) throws Exception {
        sqlSession.delete(NAMESPACE + ".delete", article_no);
    }

    @Override
    public List<ArticleVO> listAll() throws Exception {
        return sqlSession.selectList(NAMESPACE + ".listAll");
    }
}
```

### 5. articleMappler.xml

경로 : src/main/resources/mappers/aticle 에 articleMapper.xml 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="mc.sn.KEPL.mappers.article.ArticleMapper">

    <insert id="create" useGeneratedKeys="true" keyProperty="article_no">
        INSERT INTO tb_article (
            article_no
        	, title
            , content
            , writer
            , regdate
        	, viewcnt
        ) VALUES (
            #{article_no}
        	, #{title}
            , #{content}
            , #{writer}
			, #{regDate}
        	, #{viewCnt}
        )
    </insert>
	
    <select id="read" resultMap="ArticleResultMap">
        SELECT
            article_no
        	, title
            , content
            , writer
            , regdate
        	, viewcnt
        FROM
            tb_article
        WHERE article_no = #{article_no}
    </select>


    <update id="update">
        UPDATE tb_article
        SET
            title = #{title}
            , content = #{content}
        WHERE
            article_no = #{article_no}
    </update>

    <delete id="delete">
        DELETE FROM tb_article
        WHERE article_no = #{article_no}
    </delete>

    <select id="listAll" resultType="ArticleVO">
        <![CDATA[
        SELECT
            article_no,
            title,
            content,
            writer,
            regdate,
            viewcnt
        FROM tb_article
        WHERE article_no > 0
        ORDER BY article_no DESC, regdate DESC
       
        ]]>
    </select>
    
    <resultMap id="ArticleResultMap" type="ArticleVO">
        <id property="article_no" column="article_no"/>
        <result property="title" column="title" />
        <result property="content" column="content" />
        <result property="writer" column="writer" />
        <result property="regDate" column="regdate" />
        <result property="viewCnt" column="viewcnt" />
    </resultMap>
    
</mapper>
    
```

### 6. ArticleService.java (Interface)

경로 : src/main/java 에서 Servicle패키지 안에 생성

```java
public interface ArticleService {

    void create(ArticleVO articleVO) throws Exception;

    ArticleVO read(Integer article_no) throws Exception;

    void update(ArticleVO articleVO) throws Exception;

    void delete(Integer article_no) throws Exception;

    List<ArticleVO> listAll() throws Exception;
    
}
```

### 7.ArticleServiceImpl.java

ArticleService 인터페이스를 구현한 ArticleServiceImpl 생성

```java
@Service
public class ArticleServiceImpl implements ArticleService {

    private final ArticleDAO articleDAO;

    @Inject
    public ArticleServiceImpl(ArticleDAO articleDAO) {
        this.articleDAO = articleDAO;
    }
    
    @Override
    public void create(ArticleVO articleVO) throws Exception {
        articleDAO.create(articleVO);
    }
    
    @Override
    public ArticleVO read(Integer article_no) throws Exception {
        return articleDAO.read(article_no);
    }
    
    @Override
    public void update(ArticleVO articleVO) throws Exception {
        articleDAO.update(articleVO);
    }

    @Transactional
    @Override
    public void delete(Integer article_no) throws Exception {
        articleDAO.delete(article_no);
    }

    @Override
    public List<ArticleVO> listAll() throws Exception {
        return articleDAO.listAll();
    }
}
```

### 8.ArticleController 

경로 : src/main/java 에서 Controller패키지 안에 생성

```java
@Controller
@RequestMapping("/article")
public class ArticleController {

    private static final Logger logger = LoggerFactory.getLogger(ArticleController.class);

    private final ArticleService articleService;

    @Inject
    public ArticleController(ArticleService articleService) {
        this.articleService = articleService;
    }
    
 // 등록 페이지 이동
    @RequestMapping(value = "/write", method = RequestMethod.GET)
    public String writeGET() {

        logger.info("write GET...");

        return "/article/write";
    }
    
 // 등록 처리
    @RequestMapping(value = "/write", method = RequestMethod.POST)
    public String writePOST(ArticleVO articleVO,
                            RedirectAttributes redirectAttributes) throws Exception {

        logger.info("write POST...");
        logger.info(articleVO.toString());
        articleService.create(articleVO);;
        redirectAttributes.addFlashAttribute("msg", "regSuccess");

        return "redirect:/article/list";
    }
    
 // 목록 페이지 이동
    @RequestMapping(value = "/list", method = RequestMethod.GET)
    public String list(Model model) throws Exception {

        logger.info("list ...");
        model.addAttribute("articles", articleService.listAll());

        return "/article/list";
    }
    
    @RequestMapping(value = "/listCriteria", method = RequestMethod.GET)
    public String listCriteria(Model model, Criteria criteria) throws Exception {
        logger.info("listCriteria ...");
        model.addAttribute("articles", articleService.listCriteria(criteria));
        return "/article/list_criteria";
    }
    
 // 조회 페이지 이동
    @RequestMapping(value = "/read", method = RequestMethod.GET)
    public String read(@RequestParam("article_no") int article_no,
                       Model model) throws Exception {

        logger.info("read ...");
        model.addAttribute("article", articleService.read(article_no));

        return "/article/read";
    }
    
 // 수정 페이지 이동
    @RequestMapping(value = "/modify", method = RequestMethod.GET)
    public String modifyGET(@RequestParam("article_no") int article_no,
                            Model model) throws Exception {

        logger.info("modifyGet ...");
        model.addAttribute("article", articleService.read(article_no));

        return "/article/modify";
    }
    
 // 수정 처리
    @RequestMapping(value = "/modify", method = RequestMethod.POST)
    public String modifyPOST(ArticleVO articleVO,
                             RedirectAttributes redirectAttributes) throws Exception {

        logger.info("modifyPOST ...");
        articleService.update(articleVO);
        redirectAttributes.addFlashAttribute("msg", "modSuccess");

        return "redirect:/article/list";
    }
    
 // 삭제 처리
    @RequestMapping(value = "/remove", method = RequestMethod.POST)
    public String remove(@RequestParam("article_no") int article_no,
                         RedirectAttributes redirectAttributes) throws Exception {

        logger.info("remove ...");
        articleService.delete(article_no);
        redirectAttributes.addFlashAttribute("msg", "delSuccess");

        return "redirect:/article/list";
    }

}    
```





