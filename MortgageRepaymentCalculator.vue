extends ../templates/base

block title
    title Инструкция
    meta(name="description", content="Описание страницы")

block content
    .pages-wrap
        .pages-content.js--page-content
            include ../includes/header-personal-account
            +header(0)
            main
                .container
                    .block
                        p Привет
