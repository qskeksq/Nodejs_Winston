# nodejs_winston
nodejs 로깅 모듈 winston

- Basics

    ```javaScript
    var winston = require('winston');
    var winstonDaily = require('winston-daily-rotate-file');
    var moment = require('moment');

    function timeStampFormat(){
        return moment.format('YYYY-MM-DD HH:mm:ss.SSS zz');
    }

    var logger = new winston.Logger({
        transports : 
        [
            new winstonDaily(
                {
                    name : 'info-file',
                    filename : './log/server',
                    datePattern : '_yyyy-MM-dd.log',
                    colorize : false,
                    maxsize : 50000000,
                    maxFiles : 1000,
                    level : 'info',
                    showLevel : true,
                    json : false,
                    timestamp : timeStampFormat
                }
            ),
            new winston.transports.Console(
                {
                    name : 'debug-console',
                    colorize : true,
                    level : 'debug',
                    showLevel : true,
                    json : false,
                    timestamp : timeStampForamt
                }
            )
        ],
        exceptionHandlers : [
            new winstonDaily(
                {
                    name : 'exception-file',
                    filename : './log/exception',
                    datePattern : '_yyyy-MM-dd.log',
                    colorize : false,
                    maxsize : 50000000,
                    maxFiles : 1000,
                    level : 'error',
                    showLevel : true,
                    json : false,
                    timestamp : timeStampFormat
                }
            ),
            new winston.transports.Console(
                {
                    name : 'exception-console',
                    colorize : true,
                    level : 'debug',
                    showLevel : true,
                    json : false,
                    timestamp : timeStampForamt
                }
            )
        ]

    });
    ```


 - constructor
    - winston-daily-rotate-file'을 두 개 사용하려면 따로 설정이 필요한 듯 하다
  
    ```javaScript
    var winston = require('winston');
    const tsFormat = () => (new Date()).toLocaleTimeString();
    const logger = new winston.Logger({
        transports :
                    [
                        new winston.transports.Console({
                            level : 'debug',
                            colorize : true,
                            timestamp : tsFormat
                        }),
                        new (require('winston-daily-rotate-file'))({
                            level: 'info',
                            filename: './log/out.log',
                            timestamp: tsFormat,
                            datePattern: 'yyyy-MM-dd',
                            colorize : true,
                            maxsize: 100000,
                            prepend: true
                        }),
                        new winston.transports.File({
                            level: 'error',
                            filename: './log/err.log',
                            colorize : true,
                            maxsize: 100000,
                            timestamp: tsFormat,
                        })
                    ]
    });
    ```
    ```javaScript
    exports.info = function(msg){
        logger.debug(msg);
    }

    exports.info = function(msg){
        logger.info(msg);
    }

    exports.error = function(msg){
        logger.error(msg);
    }
    ```

- 적용

    ```javaScript
    logger.info('[1] DATABASE IS RUNNING...');
    logger.error('[1] DATABASE IS RUNNING...');
    logger.warn('[1] DATABASE IS RUNNING...');
    ```
