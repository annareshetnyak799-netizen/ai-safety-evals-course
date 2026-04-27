# Заметка по планированию Week 5

## Формулировка цели курса

Моя цель в этом курсе - развить суждение и практический навык проектирования evaluations, которые не просто технически корректны, а действительно полезны для принятия решений в области AI safety. Я хочу двигаться к работе, где смогу соединять дизайн evaluations, исследовательскую строгость и реальные вопросы deployment, особенно в ситуациях, где слабые benchmarks или поверхностные метрики могут создавать ложную уверенность в безопасности.

### Формат по шаблону задания

- Цель в одном предложении: перейти из ML-инженерии в AI safety evals и научиться проектировать evaluations, которые измеряют целевое safety-свойство, а не его удобную proxy-метрику.
- Какой результат я хочу получить на выходе: понятную учебную и исследовательскую траекторию, собранную карту ключевых идей курса и практическую основу для дальнейшей работы в agentic / safety evaluations.
- Почему это для меня важно: я хочу уметь отличать evaluation, которая действительно даёт полезный сигнал для safety и deployment decisions, от evaluation, которая только выглядит убедительно и создаёт ложную уверенность.

## Что Week 5 предлагает мне понять

Эта неделя посвящена надёжным AI safety evaluations и тем видам evaluation work, которые ещё только предстоит построить. Ключевой вопрос здесь не просто в том, запускается ли evaluation, а в том, достаточно ли она осмысленна, чтобы на её основе делать выводы о безопасности, deployment и policy.

Неделя разворачивается на трёх уровнях:

1. Benchmarks в AI safety отличаются от обычных capability benchmarks.
   Безопасность нормативна, социотехнически встроена и хуже сводится к фиксированным prompts и единичным score.
2. Наука об evals имеет значение.
   Если evaluations используются в high-stakes settings, им нужны более чёткие объекты измерения, лучшее coverage, reproducibility и более зрелая статистическая культура.
3. Research taste имеет значение.
   Даже хороших framework-ов недостаточно, если исследователь снова и снова выбирает лёгкие задачи вместо действительно важных.

## Вопросы, на которые должен отвечать мой mindmap

- Что делает AI safety evaluation отличной от обычного benchmark design?
- Почему фиксированных prompts и одного aggregate score часто недостаточно для safety?
- Что делает evaluation достаточно зрелой для high-stakes decisions?
- Какие основные failure modes бывают у слабых safety evals?
- Как coverage, reproducibility и статистическая строгость связаны с trustworthiness?
- Как research taste влияет на то, какие safety questions вообще оказываются измеренными?

## Обязательные тексты Week 5

- *How Should AI Safety Benchmarks Benchmark Safety?*
- *We Need A ‘Science of Evals’*
- *You and Your Research*

## Что именно я изучила на курсе и как это связано между собой

- Week 1: основы работы с `Inspect AI`, структура task-ов, solver-ов и scorer-ов
- Week 2: статистическая интерпретация результатов, uncertainty, confidence intervals и limits of small-sample conclusions
- Week 3: custom evaluators, LLM judges, distinction between benchmark artifact and measurement instrument
- Week 4: agent evaluation, tools, scaffolding, cost of multi-step trajectories и importance of dev/test discipline
- Week 5: reliable AI safety evaluations, science of evals, benchmark maturity, research taste и limits of safety benchmarking

Связь между неделями я вижу так:

- Week 1 дала базовую техническую рамку, как вообще устроены eval pipelines
- Week 2 добавила статистическую дисциплину и осторожность в интерпретации результатов
- Week 3 показала, что evaluator design сам по себе является объектом анализа, а не только “обвязкой”
- Week 4 расширила это на agents, tools и system-level behavior
- Week 5 подняла вопрос ещё выше: когда evaluation вообще заслуживает доверия в контексте safety, deployment и policy

## Предлагаемая структура mindmap

### Центральный узел

- Надёжные AI safety evaluations

### Ветка 1: Почему safety evals отличаются

- безопасность нормативна
- безопасность социотехнически встроена
- harms зависят от контекста
- fixed prompts ограничены
- единичные scores могут скрывать важную вариативность

### Ветка 2: Science of evals

- объект измерения
- construct validity
- coverage
- reproducibility
- статистическая дисциплина
- неопределённость и confidence intervals
- зрелость benchmark-а

### Ветка 3: Research taste

- важные задачи против лёгких задач
- ложный комфорт от удобных метрик
- выбор того, что действительно важно
- долгосрочное исследовательское суждение

### Ветка 4: Практические последствия

- решения о deployment
- policy relevance
- governance implications
- необходимость более качественных safety benchmarks
- необходимость более прозрачной отчётности и границ интерпретации

## Что я отложила на потом

- более глубокое чтение дополнительных текстов и benchmark-ов по multi-agent evaluation
- более подробное изучение реальных safety benchmark suites вроде METR Task Suite, AgentBench и ControlArena
- дальнейшую проработку вопроса о том, какие safety properties вообще можно надёжно operationalize

## Что я хочу показать в финальной сдаче Week 5

- что я понимаю, почему safety evaluation сложнее, чем обычная capability evaluation
- что я умею связывать benchmark design с measurement validity и research judgment
- что мой mindmap - это не просто визуальное summary, а структурная модель аргумента недели

## Короткая рефлексия

Самый важный сдвиг этой недели - переход от вопроса «как мне запустить evaluation?» к вопросу «как мне понять, заслуживает ли эта evaluation влияния на реальные решения?». Из-за этого неделя ощущается уже не как упражнение на tooling, а как проверка зрелости evaluation-мышления.
