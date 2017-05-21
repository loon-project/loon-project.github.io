<h1 align="center">映射</h1>

> 映射功能在另外一个npm 库叫做 `loon-data`。
> 这个功能还没有准备好在生产环境用。

## 1 什么是映射？

映射能够处理模型与其他模型关系，
通常用作处理关系型数据库返回结果到模型的转化。


## 2 ResultMapping

一个复杂的例子:

```typescript
class Comment {

    @Property()
    public id: number;

    @Property()
    public postId: number;

    @Property()
    public name: string;

    @Property()
    public comment: string;
}

class Tag {

    @Property()
    public id: number;
}

class Author {

    @Property()
    public id: number;

    @Property()
    public username: string;

    @Property()
    public password: string;

    @Property()
    public email: string;

    @Property()
    public bio: string;

    @Property('favourite_section')
    public favouriteSection: string;
}

class Post {

    @Property()
    public id: number;

    @Property()
    public subject: string;

    public author: Author;

    public comments: Comment[];

    public tags: Tag[];
}

class Blog {

    @Property()
    public id: number;

    @Property()
    public title: string;

    public author: Author;

    public posts: Post[];
}

const AuthorMap: AssociationMap = {
    property: 'author',
    type: Author,
    prefix: 'author_'
};

const BlogMap: DataMap = {

    type: Blog,

    prefix: 'blog_',

    associations: [
        AuthorMap
    ],

    collections: [
        {
            property: 'posts',
            type: Post,
            prefix: 'post_',

            associations: [
                AuthorMap
            ],

            collections: [
                {
                    property: 'comments',
                    type: Comment,
                    prefix: 'comment_'
                },

                {
                    property: 'tags',
                    type: Tag,
                    prefix: 'tag_'
                }
            ]

        }
    ]

};
/**
 * SQL:
 * select(
 *     'B.id as blog_id',
 *     'B.title as blog_title',
 *     'B.author_id as blog_author_id',
 *     'A.id as author_id',
 *     'A.username as author_username',
 *     'A.password as author_password',
 *     'A.email as author_email',
 *     'A.bio as author_bio',
 *     'A.favourite_section as author_favourite_section',
 *     'P.id as post_id',
 *     'P.blog_id as post_blog_id',
 *     'P.author_id as post_author_id',
 *     'P.created_on as post_created_on',
 *     'P.section as post_section',
 *     'P.subject as post_subject',
 *     'P.draft as post_draft',
 *     'P.body as post_body',
 *     'C.id as comment_id',
 *     'C.post_id as comment_post_id',
 *     'C.name as comment_name',
 *     'C.comment as comment_text',
 *     'T.id as tag_id',
 *     'T.name as tag_name'
 *   )
 *   .from('blogs1 as B')
 *   .leftOuterJoin('authors1 as A', 'A.id', 'B.author_id')
 *   .leftOuterJoin('posts1 as P', 'P.blog_id', 'B.id')
 *   .leftOuterJoin('comments1 as C', 'C.post_id', 'P.id')
 *   .leftOuterJoin('posts_tags1 as PT', 'PT.post_id', 'P.id')
 *   .leftOuterJoin('tags1 as T', 'T.id', 'PT.tag_id')
 *   .where({'B.id': id})
 *
 * Tables:
 *   CREATE TABLE `authors1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `username` varchar(255) DEFAULT NULL,
 *     `password` varchar(255) DEFAULT NULL,
 *     `email` varchar(255) DEFAULT NULL,
 *     `bio` varchar(255) DEFAULT NULL,
 *     `favourite_section` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;
 *
 *   CREATE TABLE `blogs1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `title` varchar(255) DEFAULT NULL,
 *     `author_id` int(11) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `comments1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `post_id` int(11) DEFAULT NULL,
 *     `name` varchar(255) DEFAULT NULL,
 *     `comment` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=133 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `posts_tags1` (
 *     `post_id` int(11) DEFAULT NULL,
 *     `tag_id` int(11) DEFAULT NULL
 *   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

 *   CREATE TABLE `posts1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `blog_id` int(11) DEFAULT NULL,
 *     `author_id` int(11) DEFAULT NULL,
 *     `created_on` varchar(255) DEFAULT NULL,
 *     `section` varchar(255) DEFAULT NULL,
 *     `subject` varchar(255) DEFAULT NULL,
 *     `draft` tinyint(1) DEFAULT NULL,
 *     `body` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `tags1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `name` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8;

 *   CREATE TABLE `users1` (
 *     `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
 *     `username` varchar(255) DEFAULT NULL,
 *     `hashedPassword` varchar(255) DEFAULT NULL,
 *     PRIMARY KEY (`id`)
 *   ) ENGINE=InnoDB AUTO_INCREMENT=41 DEFAULT CHARSET=utf8;
 */

class BlogRepository {

    @ResultMapping(BlogMap)
    public queryBlog() {
        return [
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 130,
                "comment_post_id": 87,
                "comment_name": "1",
                "comment_text": "c1",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 131,
                "comment_post_id": 87,
                "comment_name": "2",
                "comment_text": "c2",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 132,
                "comment_post_id": 87,
                "comment_name": "3",
                "comment_text": "c3",
                "tag_id": 87,
                "tag_name": "great"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 130,
                "comment_post_id": 87,
                "comment_name": "1",
                "comment_text": "c1",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 131,
                "comment_post_id": 87,
                "comment_name": "2",
                "comment_text": "c2",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 87,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-07",
                "post_section": "s1",
                "post_subject": "first blog",
                "post_draft": 1,
                "post_body": "body 1",
                "comment_id": 132,
                "comment_post_id": 87,
                "comment_name": "3",
                "comment_text": "c3",
                "tag_id": 88,
                "tag_name": "cloud"
            },
            {
                "blog_id": 44,
                "blog_title": "great blog",
                "blog_author_id": 87,
                "author_id": 87,
                "author_username": "Jack",
                "author_password": "password",
                "author_email": "jack@gmail.com",
                "author_bio": null,
                "author_favourite_section": "NO.1",
                "post_id": 88,
                "post_blog_id": 44,
                "post_author_id": 87,
                "post_created_on": "2077-07-17",
                "post_section": "s2",
                "post_subject": "second blog",
                "post_draft": 0,
                "post_body": "body 2",
                "comment_id": null,
                "comment_post_id": null,
                "comment_name": null,
                "comment_text": null,
                "tag_id": 87,
                "tag_name": "great"
            }
        ];
    }
}

const repo = new BlogRepository();
const blog = repo.queryBlog();

blog instanceof Blog # => true
```

在这个例子中， `Blog` 与 `Post` 有一对多的关系, 与 `Author` 有一对一的关系。
`Post` 与 `Author` 有一对一的关系, 与 `Tag` 和 `Comment` 有一对多的关系。

`ResultMapping` 会返回一个模型的映射结果, 他接受一个实现 `DataMap` 接口的参数。<br>
`DataMap` 有一个必选项是 `type`, 它定义了需要映射的模型。 <br>
`prefix` 是可选项, 为了防止在复杂的数据关系中有属性名相同冲突。 <br>
`associations` 是可选项，为了定义模型中 `一对一` 关系，
它接受一个 `AssociationMap` 数组, `AssociationMap` 继承自 `DataMap`, 并扩展了一个必选项叫 `property`,
用来决定关系映射到那个模型的属性上。 <br>
`collections` 是可选项, 为了定义模型中 `一对多`关系，
它接受一个`CollectionMap` 数组, `CollectionMap` 用法和 `AssociationMap` 一样。

> 现在在 `DataMap` 必须要有一个 `id` 的属性来区别数据与数据的不同。


## 3 ResultsMapping

`ResultsMapping` 和 `ResultMapping` 几乎一样，唯一的分别是它返回模型的数组。


## 4 EntityMapping

```typescript
class Developer {

    @Property()
    public name: string;

    @Property({name: "skillsContent"})
    public skills: string[];
}

class PeopleRepository {

    @EntityMapping()
    public insertDeveloper(developer: Developer) {
        return developer; // developer is now an object
    }

}
```
使用了 `@EntityMapping` 注解的方法会自动的转化方法参数中的类到 `Object`。
