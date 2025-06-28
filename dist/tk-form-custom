!(function (t) {
    let e = "custom-form",
        r = "custom-form-submit",
        o = (t = {}) => {
            e = t.formClass || e;
            r = t.customSubmitClass || r;
            t_onReady(() => {
                t_onFuncLoad("t_zeroForms__onReady", () => {
                    n();
                });
            });
        },
        n = () => {
            const forms = document.querySelectorAll('.' + e);
            if (forms.length === 0) {
                console.error("[TKFORM] Не найдено ни одной формы с классом", e);
                return;
            }

            const blocks = new Set();
            forms.forEach((form) => {
                const block = form.closest(".t-rec");
                if (block) blocks.add(block);
            });

            if (blocks.size === 0) {
                console.error("[TKFORM] Не найдено ни одного зеро блока с формами");
                return;
            }

            i(forms);
            s(blocks);
            a(forms);
        },
        i = (t) => {
            t.forEach((form) => {
                const btn = form.querySelector(".tn-form__submit");
                if (btn) btn.remove();
            });
        },
        a = (t) => {
            t.forEach((form) => {
                const inputs = form.querySelectorAll(
                    ".t-input:not(.t-inputquantity):not(.t-input-phonemask__wrap):not(.t-input-phonemask):not(.t-input__own-answer)"
                );
                inputs.forEach((input) => {
                    input.addEventListener("blur", function (e) {
                        e.target.value
                            ? e.target.classList.add("t-input_has-content")
                            : e.target.classList.remove("t-input_has-content");
                    });
                });
            });
        },
        s = (t) => {
            t.forEach((block) => {
                const artboard = block.querySelector(".t396__artboard");
                if (!artboard) {
                    console.error("[TKFORM] Не найден элемент t396__artboard в блоке:", block);
                    return false;
                }

                const forms = block.querySelectorAll('.' + e);
                if (forms.length === 0) {
                    console.error(`[TKFORM] Не найдено ни одной формы с классом ${e} в блоке`, block);
                    return false;
                }

                const submitBtn = block.querySelector('.' + r);
                if (!submitBtn) {
                    console.error(`[TKFORM] Не найдено кнопки submit с классом ${r} в блоке`, block);
                    return false;
                }

                const formId = artboard.dataset.artboardRecid
                    ? "tk-form" + artboard.dataset.artboardRecid
                    : "tk-form" + Math.floor(1e5 + 9e5 * Math.random());

                const wrapper = document.createElement("div");
                wrapper.innerHTML = `
                    <form 
                        class="t-form t-form_inputs-total_2 js-form-proccess" 
                        id="${formId}" 
                        name="form778879734" 
                        action="https://forms.tildacdn.com/procces/ " 
                        method="POST" 
                        role="form"
                        data-formactiontype="2"
                        data-inputbox=".t-input-group"
                        data-success-callback="t396_onSuccess"
                        data-success-popup="y"
                        data-error-popup="y">
                    </form>
                `;
                const newForm = wrapper.firstElementChild;

                forms.forEach((formWrapper) => {
                    const originalForm = formWrapper.querySelector("form");
                    if (!originalForm) {
                        return console.error("[TKFORM] Не найдена форма в элементе", formWrapper);
                    }

                    const div = document.createElement("div");
                    [...originalForm.attributes].forEach((attr) =>
                        div.setAttribute(attr.name, attr.value)
                    );
                    div.append(...originalForm.cloneNode(true).childNodes);
                    originalForm.replaceWith(div);
                    newForm.appendChild(formWrapper);
                });

                newForm.appendChild(submitBtn);
                artboard.appendChild(newForm);
                l(newForm, submitBtn);
            });
        },
        l = (form, btn) => {
            if (!form) {
                console.error("[TKFORM] Не найдено комбинированной формы");
                return false;
            }
            if (!btn) {
                console.error(`[TKFORM] Не найдено кнопки submit в форме`, form);
                return false;
            }

            btn.setAttribute("type", "submit");
            btn.setAttribute("tabindex", "0");
            btn.setAttribute("onKeyDown", "tkForm.handleSubmitKeyDown(event)");
            const style = btn.getAttribute("style");
            btn.setAttribute("style", style + " cursor: pointer;");
            btn.classList.add("t-submit");

            // Обработка клика или клавиши Enter/Space
            btn.addEventListener("click", function (e) {
                e.preventDefault();
                e.stopPropagation();

                window.tildaForm.hideErrors(form);
                const errors = window.tildaForm.validate(form);

                if (errors.length > 0) {
                    window.tildaForm.showErrors(form, errors);
                    return;
                }

                // Отправляем форму "натурально"
                form.submit();
            });

            btn.addEventListener("keydown", function (e) {
                tkForm.handleSubmitKeyDown(e);
            });

            t_onReady(function () {
                setTimeout(function () {
                    if (window.t_upwidget__init) {
                        t_zeroForms__onFuncLoad("t_upwidget__init", () =>
                            btn.classList.remove("t-submit")
                        );
                    } else {
                        btn.classList.remove("t-submit");
                    }
                }, 500);
            });
        };

    t.tkForm = {
        init: o,
        handleSubmitKeyDown: function (e) {
            if (e.keyCode === 13 || e.keyCode === 32) {
                e.preventDefault();
                e.target.dispatchEvent(new Event("click"));
            }
        },
    };
})(window);
