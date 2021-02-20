<template>
    <section class="card-contacts">
        <div class="item-frame item-half">
            <div class="item-frame__block">
                <p class="item-frame__header">
                    Контактная информация
                </p>
                <a class="item-frame__link" title="Авторизоваться"
                   href="http://sotbisite.beget.tech//auth/">Авторизоваться</a>
            </div>
            <div class="card-contacts__form ">
                <div class="form__item-fio">
                    <div class="form__item form__item__third">
                        <input
                                class="card-contacts__field card-contacts__field--mini"
                                name="NAME"
                                type="text"
                                id="NAME"
                                value=""
                                v-model="order.person.name"
                                @focus="labels.name = true"
                                @blur="labels.name = false" @keypress="symbolCheck($event)"
                        >
                        <label for="NAME" class="form-label" :class="{'form-label--active': labels.name || order.person.name !== ''}">Имя*</label>
                    </div>
                    <div class="form__item form__item__third">
                        <input class="card-contacts__field card-contacts__field--mini" name="FIO" type="text"
                               id="FIO" value="" v-model="order.person.lastName" @focus="labels.lastName = true"
                               @blur="labels.lastName = false" @keypress="symbolCheck($event)">
                        <label for="FIO" class="form-label"
                               :class="{'form-label--active': labels.lastName || order.person.lastName !== ''}">Фамилия*</label>
                    </div>
                    <div class="form__item form__item__third">
                        <input class="card-contacts__field card-contacts__field--mini" name="SECOND_NAME"
                               id="SECOND_NAME" value="" type="text" v-model="order.person.fatherName"
                               @focus="labels.fatherName = true" @blur="labels.fatherName = false" @keypress="symbolCheck($event)">
                        <label for="SECOND_NAME" class="form-label" :class="{'form-label--active': labels.fatherName || order.person.fatherName !== ''}">Отчество</label>
                    </div>
                </div>
                <div class="form__item-contacts">
                    <div class="form__item form__item__second">
                        <input class="card-contacts__field card-contacts__field--half card-contacts__field--phone"
                               name="PHONE" id="PHONE" value="" type="tel" v-model="order.person.phone"
                               @focus="labels.phone = true" @blur="labels.phone = false">
                        <label for="PHONE" class="form-label"
                               :class="{'form-label--active': labels.phone || order.person.phone !== ''}">Телефон*</label>
                    </div>

                    <div class="form__item form__item__second">
                        <input class="card-contacts__field card-contacts__field--half" name="EMAIL" id="EMAIL"
                               value="" type="email" data-msg-email="Укажите корректный e-mail адрес"
                               v-model="order.person.email" @focus="labels.email = true" @blur="labels.email = false">
                        <label for="EMAIL" class="form-label"
                               :class="{'form-label--active': labels.email || order.person.email !== ''}">E-mail*</label>
                    </div>
                </div>
                <div class="form__item-message">
                    <div class="form__item">
                        <textarea class="card-contacts__field" id="COMMENTARY" name="COMMENTARY"
                                  v-model="order.person.message" @focus="labels.message = true"
                                  @blur="labels.message = false" @keypress="symbolCheck($event)"></textarea>
                        <label for="COMMENTARY" class="form-label delivery__placeholder"
                               :class="{'form-label--active': labels.message || order.person.message !== ''}">Комментарий</label>
                    </div>
                </div>
            </div>
        </div>

        <div class="item-frame item-half item-frame--card-delivery">


            <h2 class="oformit_none " style="display:none;">Минимальная сумма заказа - 500 руб.</h2>


            <p class="item-frame__header">Получение товара<span data-tooltip-content="#tooltip_content"
                                                                class="tooltip delivery-tooltip">?</span>
            </p>

            <div class="tooltip_templates">
                        <span id="tooltip_content">
                            <strong>Доставка:</strong> СПб до КАД, Авиагородок, Парголово
                            <br/>
                            <br/>
                            <a href="http://sotbisite.beget.tech//delivery/" rel="nofollow"
                               target="_blank">Подробнее</a>
                        </span>
            </div>
            <div class="item-frame__reserve">
                <p>Товар для самовывоза бронируется на 24 часа</p>
            </div>
            <div class="card-delivery__form" id="cardDeliveryList">

                <label class="delivery__label" v-for="deliveryMethod in deliveryData" :key="deliveryMethod.id">
                    <div class="card-delivery__radio-decor"
                         :class="{
                            'card-delivery__radio-decor--active': order.delivery.id === deliveryMethod.id
                         }"/>
                    <input id="label_3" class="card-delivery__radio" type="radio"
                           name="DELIVERY_ID" v-model="order.delivery.id" :value="deliveryMethod.id"
                           @click="setDeliveryCost(deliveryMethod.cost)">
                    {{deliveryMethod.name}}
                </label>
            </div>

            <div class="delivery__option">

                <div class="delivery__novalue" v-show="order.delivery.id === ''">
                    <span>Способ получения не выбран!</span>
                </div>

                <div class="modal" id="modal" :class="{open: isModalOpen}">

                    <div class="modal-content">
                        <span class="close" @click="isModalOpen=false">&times;</span>


                        <div class="tablist-availability tablist-availability--modal">
                            <div class="availability__list">
                                <div class="availability__sorting">
                                    <div>Сортировка</div>
                                    <div class="sorting__choice">
                                        <a class="sorting__district sorting__choice--active"
                                           href="http://sotbisite.beget.tech/#">по району</a>
                                        <span> / </span>
                                        <a class="sorting__metro" href="http://sotbisite.beget.tech/#">по
                                            станции метро</a>
                                    </div>
                                </div>

                                <div class="availability__items" id="test" style="display: block">
                                    <div class="availability__district">

                                        <div class="district__title">
                                            Адмиралтейский район
                                        </div>

                                        <div class="district__item" data-coord="59.904126,30.312437"
                                             data-id="41168">
                                            <div class="item__address item__address--allday">
                                                Московский пр., д. 73, к.5
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Фрунзенская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.928179530468,30.319279109787" data-id="319">
                                            <div class="item__address item__address--allday">
                                                Спасский пер., д. 14/35
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Садовая</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Сенная пл.</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Спасская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Василеостровский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.960146284455,30.228741987102" data-id="13486">
                                            <div class="item__address item__address--allday">
                                                Морская наб., д. 39, к. 3
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--green"><span
                                                        class="metro metro3">Приморская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Всеволожский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.908496301525,30.516329828156" data-id="13468">
                                            <div class="item__address item__address--allday">
                                                Европейский пр., д.18, к.2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.900316145575,30.512587609786" data-id="13467">
                                            <div class="item__address item__address--allday">
                                                Европейский пр., д.8
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Выборгский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.040959894865,30.324052745369" data-id="13479">
                                            <div class="item__address item__address--allday">
                                                пр. Энгельса, д. 126 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.047911108444,30.321781598217" data-id="13488">
                                            <div class="item__address item__address--allday">
                                                ул. Асафьева, д. 3
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Калининский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.013033205419,30.395001399475" data-id="13482">
                                            <div class="item__address item__address--allday">
                                                пр. Науки, д. 19, к. 2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Академическая</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.033310952584,30.426563508598" data-id="13487">
                                            <div class="item__address item__address--allday">
                                                Проспект Просвещения, д. 91 (Киришская ул., д. 4)
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Гражданский пр.</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Кировский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.852400075018,30.228200646835" data-id="13491">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д.104
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Автово</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.833484964891,30.199152900139" data-id="13489">
                                            <div class="item__address item__address--allday">
                                                пр. Ветеранов, д. 109, к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Проспект Ветеранов</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Красногвардейский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.944327531276,30.491713563824" data-id="13485">
                                            <div class="item__address item__address--allday">
                                                пр. Наставников, д. 19
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Ладожская</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Красносельский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.857434686271,30.204546737434" data-id="13475">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д. 88
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.859636277641,30.190424751312" data-id="13471">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д.78, к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Московский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.855157730582,30.322275776127" data-id="13490">
                                            <div class="item__address item__address--allday">
                                                Московский пр., д. 208
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Московская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Невский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.908631048246,30.456305469264" data-id="40927">
                                            <div class="item__address item__address--allday">
                                                Подвойского 6/5
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.925865672872,30.473648913353" data-id="13484">
                                            <div class="item__address item__address--allday">
                                                ул. Чудновского, д. 19 (Российский пр., д. 7)
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Петроградский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.961138316536,30.314767944775" data-id="13476">
                                            <div class="item__address item__address--allday">
                                                Б. Монетная ул., д. 10
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Горьковская</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.968649578402,30.302820296959" data-id="13478">
                                            <div class="item__address item__address--allday">
                                                Чкаловский пр., д. 60
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Чкаловская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Приморский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.002301935887,30.243271206676" data-id="13480">
                                            <div class="item__address item__address--allday">
                                                Богатырский пр., д. 28
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.006132707378,30.264543738179" data-id="43303">
                                            <div class="item__address item__address--allday">
                                                Испытателей пр., д. 30/2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item" data-coord="60.004244,30.295135"
                                             data-id="13483">
                                            <div class="item__address item__address--allday">
                                                Коломяжский пр. 26
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.037720798679,30.22563214418" data-id="40926">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр. 67
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.020749730412,30.242872714958" data-id="13481">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр., д. 34 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.031561374108,30.242866464943" data-id="13469">
                                            <div class="item__address item__address--allday">
                                                пр. Королёва, д. 61
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.989558312588,30.201422674274" data-id="13470">
                                            <div class="item__address item__address--allday">
                                                Савушкина ул., д.143
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--green"><span
                                                        class="metro metro3">Беговая</span></div>
                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="60.00261554371,30.208657803566" data-id="13473">
                                            <div class="item__address item__address--allday">
                                                Туристская ул., д.28 к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>

                                            </div>
                                        </div>

                                        <div class="district__title">
                                            Фрунзенский район
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.868989775903,30.375887118385" data-id="13472">
                                            <div class="item__address item__address--allday">
                                                Белы Куна, д.1, к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Международная</span></div>

                                            </div>
                                        </div>

                                        <div class="district__item"
                                             data-coord="59.835858371158,30.380329113421" data-id="13477">
                                            <div class="item__address item__address--allday">
                                                Дунайский пр., д. 34/16
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Купчино</span></div>
                                            </div>
                                        </div>

                                    </div>
                                    <div class="availability__metro">
                                        <div class="district__title">
                                            <div class="item__station item__station--red"><span
                                                    class="metro metro1">Автово</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.852400075018,30.228200646835">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д.104
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Автово</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--red"><span
                                                    class="metro metro1">Академическая</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.013033205419,30.395001399475">
                                            <div class="item__address item__address--allday">
                                                пр. Науки, д. 19, к. 2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Академическая</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--green"><span
                                                    class="metro metro3">Беговая</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.989558312588,30.201422674274">
                                            <div class="item__address item__address--allday">
                                                Савушкина ул., д.143
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--green"><span
                                                        class="metro metro3">Беговая</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Горьковская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.961138316536,30.314767944775">
                                            <div class="item__address item__address--allday">
                                                Б. Монетная ул., д. 10
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Горьковская</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--red"><span
                                                    class="metro metro1">Гражданский пр.</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.033310952584,30.426563508598">
                                            <div class="item__address item__address--allday">
                                                Проспект Просвещения, д. 91 (Киришская ул., д. 4)
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Гражданский пр.</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--purple"><span
                                                    class="metro metro5">Комендантский пр.</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.002301935887,30.243271206676">
                                            <div class="item__address item__address--allday">
                                                Богатырский пр., д. 28
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.006132707378,30.264543738179">
                                            <div class="item__address item__address--allday">
                                                Испытателей пр., д. 30/2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.037720798679,30.22563214418">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр. 67
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.020749730412,30.242872714958">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр., д. 34 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.031561374108,30.242866464943">
                                            <div class="item__address item__address--allday">
                                                пр. Королёва, д. 61
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.00261554371,30.208657803566">
                                            <div class="item__address item__address--allday">
                                                Туристская ул., д.28 к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Купчино</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.835858371158,30.380329113421">
                                            <div class="item__address item__address--allday">
                                                Дунайский пр., д. 34/16
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Купчино</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--yellow"><span
                                                    class="metro metro4">Ладожская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.944327531276,30.491713563824">
                                            <div class="item__address item__address--allday">
                                                пр. Наставников, д. 19
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Ладожская</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--red"><span
                                                    class="metro metro1">Ленинский проспект</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.857434686271,30.204546737434">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д. 88
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.852400075018,30.228200646835">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д.104
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Автово</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.859636277641,30.190424751312">
                                            <div class="item__address item__address--allday">
                                                Ленинский пр., д.78, к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.833484964891,30.199152900139">
                                            <div class="item__address item__address--allday">
                                                пр. Ветеранов, д. 109, к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Проспект Ветеранов</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--purple"><span
                                                    class="metro metro5">Международная</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.868989775903,30.375887118385">
                                            <div class="item__address item__address--allday">
                                                Белы Куна, д.1, к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Международная</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Московская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.855157730582,30.322275776127">
                                            <div class="item__address item__address--allday">
                                                Московский пр., д. 208
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Московская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Озерки</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.040959894865,30.324052745369">
                                            <div class="item__address item__address--allday">
                                                пр. Энгельса, д. 126 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.047911108444,30.321781598217">
                                            <div class="item__address item__address--allday">
                                                ул. Асафьева, д. 3
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Петроградская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.961138316536,30.314767944775">
                                            <div class="item__address item__address--allday">
                                                Б. Монетная ул., д. 10
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Горьковская</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.968649578402,30.302820296959">
                                            <div class="item__address item__address--allday">
                                                Чкаловский пр., д. 60
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Чкаловская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Пионерская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.002301935887,30.243271206676">
                                            <div class="item__address item__address--allday">
                                                Богатырский пр., д. 28
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item" data-coord="60.004244,30.295135">
                                            <div class="item__address item__address--allday">
                                                Коломяжский пр. 26
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.037720798679,30.22563214418">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр. 67
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.020749730412,30.242872714958">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр., д. 34 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.031561374108,30.242866464943">
                                            <div class="item__address item__address--allday">
                                                пр. Королёва, д. 61
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--green"><span
                                                    class="metro metro3">Приморская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.960146284455,30.228741987102">
                                            <div class="item__address item__address--allday">
                                                Морская наб., д. 39, к. 3
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--green"><span
                                                        class="metro metro3">Приморская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--yellow"><span
                                                    class="metro metro4">Проспект Большевиков</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.908631048246,30.456305469264">
                                            <div class="item__address item__address--allday">
                                                Подвойского 6/5
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.944327531276,30.491713563824">
                                            <div class="item__address item__address--allday">
                                                пр. Наставников, д. 19
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Ладожская</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.925865672872,30.473648913353">
                                            <div class="item__address item__address--allday">
                                                ул. Чудновского, д. 19 (Российский пр., д. 7)
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--red"><span
                                                    class="metro metro1">Проспект Ветеранов</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.833484964891,30.199152900139">
                                            <div class="item__address item__address--allday">
                                                пр. Ветеранов, д. 109, к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Проспект Ветеранов</span></div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Ленинский проспект</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Проспект Просвещения</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.040959894865,30.324052745369">
                                            <div class="item__address item__address--allday">
                                                пр. Энгельса, д. 126 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.033310952584,30.426563508598">
                                            <div class="item__address item__address--allday">
                                                Проспект Просвещения, д. 91 (Киришская ул., д. 4)
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                                <div class="item__station item__station--red"><span
                                                        class="metro metro1">Гражданский пр.</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.047911108444,30.321781598217">
                                            <div class="item__address item__address--allday">
                                                ул. Асафьева, д. 3
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Озерки</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Проспект Просвещения</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--purple"><span
                                                    class="metro metro5">Садовая</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.928179530468,30.319279109787">
                                            <div class="item__address item__address--allday">
                                                Спасский пер., д. 14/35
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Садовая</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Сенная пл.</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Спасская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Сенная пл.</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.928179530468,30.319279109787">
                                            <div class="item__address item__address--allday">
                                                Спасский пер., д. 14/35
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Садовая</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Сенная пл.</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Спасская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--yellow"><span
                                                    class="metro metro4">Спасская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.928179530468,30.319279109787">
                                            <div class="item__address item__address--allday">
                                                Спасский пер., д. 14/35
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Садовая</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Сенная пл.</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Спасская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--purple"><span
                                                    class="metro metro5">Старая Деревня</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.002301935887,30.243271206676">
                                            <div class="item__address item__address--allday">
                                                Богатырский пр., д. 28
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.037720798679,30.22563214418">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр. 67
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.020749730412,30.242872714958">
                                            <div class="item__address item__address--allday">
                                                Комендантский пр., д. 34 к. 1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.031561374108,30.242866464943">
                                            <div class="item__address item__address--allday">
                                                пр. Королёва, д. 61
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Пионерская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="60.00261554371,30.208657803566">
                                            <div class="item__address item__address--allday">
                                                Туристская ул., д.28 к.1
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Комендантский пр.</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Старая Деревня</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--yellow"><span
                                                    class="metro metro4">Улица Дыбенко</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.908496301525,30.516329828156">
                                            <div class="item__address item__address--allday">
                                                Европейский пр., д.18, к.2
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.900316145575,30.512587609786">
                                            <div class="item__address item__address--allday">
                                                Европейский пр., д.8
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                            </div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.908631048246,30.456305469264">
                                            <div class="item__address item__address--allday">
                                                Подвойского 6/5
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Улица Дыбенко</span></div>
                                                <div class="item__station item__station--yellow"><span
                                                        class="metro metro4">Проспект Большевиков</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--blue"><span
                                                    class="metro metro2">Фрунзенская</span></div>
                                        </div>
                                        <div class="district__item" data-coord="59.904126,30.312437">
                                            <div class="item__address item__address--allday">
                                                Московский пр., д. 73, к.5
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Фрунзенская</span></div>
                                            </div>
                                        </div>
                                        <div class="district__title">
                                            <div class="item__station item__station--purple"><span
                                                    class="metro metro5">Чкаловская</span></div>
                                        </div>
                                        <div class="district__item"
                                             data-coord="59.968649578402,30.302820296959">
                                            <div class="item__address item__address--allday">
                                                Чкаловский пр., д. 60
                                            </div>
                                            <div class="item__metro">
                                                <div class="item__station item__station--blue"><span
                                                        class="metro metro2">Петроградская</span></div>
                                                <div class="item__station item__station--purple"><span
                                                        class="metro metro5">Чкаловская</span></div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="availability__choosed">
                                </div>
                            </div>
                            <div class="availability__map" id='adresses__map_card'>


                            </div>
                        </div>
                    </div>
                </div>
                <div class="delivery__samovyvoz" :class="{checked: order.delivery.id === 3}">
                    <div class="samovyvoz__inner">
                        <div class="delivery__title">
                            Аптека выдачи:
                        </div>
                        <div id="pickUpPoint">
                            Пункт самовывоза не выбран!
                        </div>

                        <button class="samovyvoz__button" id="modal-btn" @click="isModalOpen = true">
                            Выбрать аптеку
                        </button>
                    </div>

                </div>
                <div class="delivery__3hours data_18 data_17 data_19"
                     :class="{checked: order.delivery.id !== '' && order.delivery.id !== 3}">
                    <div class="delivery__title">
                        Информация о доставке
                    </div>
                    <div class="card-register-form">
                        <a class="discount__link" href="http://sotbisite.beget.tech//delivery/"
                           target="_blank" rel="noopener">Ознакомиться с условиями доставки</a>
                    </div>
                    <br>
                    <div>
                        <p style="margin: 0 0 10px;">Просим Вас обратить внимание, что при доставке оплата
                            возможно только наличными курьеру. <strong>Оплата банковскими картами
                                недоступна.</strong></p>
                    </div>
                    <div class="delivery__info">
                        <div class="delivery__field">
                            <textarea class="delivery__input" id="ADDRESS" name="ADDRESS"></textarea>
                            <label for="ADDRESS" class="delivery__placeholder ">Введите адрес
                                доставки</label>
                        </div>
                    </div>

                </div>

            </div>
            <div style="font-size: 0.8rem;"><p>Доставка товаров осуществляется из аптеки-партнера ООО
                "Аптека Фиалка" согласно <a
                        href="http://sotbisite.beget.tech//upload/iblock/980/98063b31f8788c19aea98c8d9a63aba6.pdf"
                        target="_blank">Разрешению № ДТ-78-000137</a> от 06.08.2020 г. на осуществление
                розничной торговли лекарственными препаратами для медицинского применения дистанционным
                способом. Аптека-партнер осуществляет фармацевтическую деятельность согласно <a
                        href="http://sotbisite.beget.tech//about/litsenzii/#licenses-7" target="_blank">Лицензии
                    № ЛО-78-02-003841</a> от 22.07.2020 г. на осуществление фармацевтической деятельности.
                <a href="http://sotbisite.beget.tech//delivery/" target="_blank">Условия доставки</a></p>
            </div>
        </div>
    </section>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "BasketContacts",
        props: ['isVerify', 'deliveryCost'],
        data() {
            return {
                deliveryData: [],
                labels: {
                    name: false,
                    lastName: false,
                    fatherName: false,
                    phone: false,
                    email: false,
                    message: false
                },
                order: {
                    person: {
                        name: '',
                        lastName: '',
                        fatherName: '',
                        phone: '',
                        email: '',
                        message: ''
                    },
                    delivery: {
                        id: '',
                        cost: ''
                    }
                },
                isModalOpen: false
            }
        },
        computed: {
            verify() {
                return this.order.person.name !== '' &&
                    this.order.person.lastName !== '' &&
                    this.order.person.phone !== '' &&
                    this.order.person.email !== '' &&
                    this.order.delivery.id !== ''
            },
        },
        watch: {
            verify(val) {
                this.$emit('update:isVerify', val)
            },
            'order.delivery.cost'(val) {
                this.$emit('update:deliveryCost', val)
            }

        },
        methods: {
            symbolCheck(e){
                let char = e.key; // Get the character
                if(/^[А-Яа-я]+$/.test(char)) return true; // Match with regex
                else e.preventDefault(); // If not match, don't add to input text
            },
            setDeliveryCost(cost) {
                this.order.delivery.cost = cost;
            },
            fetchDelivery() {
                axios
                    .get('delivery/')
                    .then(response => {
                        this.deliveryData = response.data
                    })
            }
        },
        created() {
            this.fetchDelivery();
        }
    }
</script>

<style scoped lang="scss">
    .card-section .card-contacts {
        flex-flow: column wrap;
    }

    .card-section .item-half {
        max-width: none;
        width: 100%;
        flex-basis: 100%;

        &:nth-child(2) {
            margin-bottom: 0;
        }
    }

    .form__item-fio, .form__item-contacts {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
    }

    .form__item-fio {
        > * {
            flex-basis: calc(100% - 20px);
            @media screen and (min-width: 577px) {
                flex-basis: calc(100% / 3 - 20px);
            }
        }
    }

    .card-section .card-contacts__form .form__item__second {
        flex-basis: calc(100% - 20px);
        @media screen and (min-width: 577px) {
            flex-basis: calc(100% / 2 - 20px);
        }
    }

    .form__item-message {
        display: flex;
        width: 100%;

        > * {
            width: 100%;
        }
        .form__item{
            margin-bottom: 0;
        }
    }

    .delivery {
        &__samovyvoz {
            &.checked {
                display: flex;
            }
        }

        &__3hours {
            &.checked {
                display: block;
            }
        }
    }

    .modal {
        &.open {
            display: block;
        }
    }

    #modal {
        padding-top: 150px;
    }

    .availability {
        &__district, &__metro {
            max-height: 560px;
        }
        &__map {
            height: 560px;
        }
    }
    .close {
        top: 10px !important;
        right: 10px;
        padding: 5px;
        border-radius: 50px;
        border: 1px solid #CE4A9A;
        line-height: 16px;
        transition: background-color .2s, color .2s;
        &:hover{
            background-color: #CE4A9A;
            color: white;
        }
    }
</style>