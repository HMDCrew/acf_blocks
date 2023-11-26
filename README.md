# ACF Blocks

## Definition acf block
```php
// is possible insert this code on functions.php
 acf_register_block_type(
	array(
		'name'            => 'prenota-ora',
		'title'           => __('Prenota Ora'),
		'supports'        => array(
			'align' => array('left', 'right', 'center', 'wide', 'full'),
		),
		'enqueue_assets'  => function () {

			$screen = function_exists('get_current_screen') ? get_current_screen() : '';

			if (
				(!empty($screen) && !empty($screen -> base) && !empty($screen -> is_block_editor)) &&
				'post' === $screen -> base && $screen -> is_block_editor
			) {
				wp_register_style('ncc_admin_css', get_stylesheet_directory_uri(). '/assets/dist/css/admin.bundle.css', array(), '0.0.1');
				wp_enqueue_style('ncc_admin_css');
			}
		},
		// 'enqueue_script'  => get_stylesheet_directory_uri() . '/assets/dist/js/admin.bundle.js',
		// 'enqueue_style'   => get_stylesheet_directory_uri() . '/assets/dist/css/admin.bundle.css',
		'render_template' => __DIR__. '/../template-parts/blocks/prenota/prenota-ora.php',
	)
);
```

## Block template
   *template-parts/blocks/prenota/prenota-ora.php*
```php
$sec = new SEC();

list(
	'form'  => $form,
	'email' => $email
) = $args;

?>

<?php if ( $form && ! empty( $email ) ) : ?>

<div class="container-prenota-ora">

	<?php if ( count( $form ) > 1 ) : ?>
	<div class="container-fluid">
		<ul class="list-unstyled multi-steps">
			<?php for ( $i = 0; $i < count( $form ); $i++ ) : ?>
				<li <?php echo ( 0 === $i ? 'class="is-active"' : '' ); ?>></li>
			<?php endfor; ?>
		</ul>
	</div>
	<?php endif; ?>

	<form action="#" method="post" class="prenota-ora">
		<input type="hidden" name="form_key" value="<?php echo base64_encode( $sec->encrypt( $email ) ); ?>">

		<?php foreach ( $form as $key => $form_row ) : ?>

			<div class="row <?php echo ( 0 === $key ? 'is-active' : '' ); ?> <?php echo ( ! empty( $block['align'] ) ? $block['align'] : '' ); ?>">
				<?php foreach ( $form_row['row'] as $column ) : ?>

					<?php if ( 'text' === $column['acf_fc_layout'] && ! empty( trim( $column['text'] ) ) ) : ?>
						<div class="item">
							<input type="text" class="form-controll" name="<?php echo apply_filters( 'text_to_attr', $column['text'] ); ?>" placeholder="<?php echo $column['text']; ?>" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>>
						</div>
					<?php endif; ?>
					

					<?php if ( 'number' === $column['acf_fc_layout'] && ! empty( trim( $column['number'] ) ) ) : ?>
						<div class="item">
							<input type="number" class="form-controll" name="<?php echo apply_filters( 'text_to_attr', $column['number'] ); ?>" placeholder="<?php echo $column['number']; ?>" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>>
						</div>
					<?php endif; ?>


					<?php if ( 'phone_number' === $column['acf_fc_layout'] && ! empty( trim( $column['phone_number'] ) ) ) : ?>
						<div class="item">
							<input type="text" class="form-controll phone-number" name="<?php echo apply_filters( 'text_to_attr', $column['phone_number'] ); ?>" placeholder="<?php echo $column['phone_number']; ?>" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>>
						</div>
					<?php endif; ?>


					<?php if ( 'email' === $column['acf_fc_layout'] && ! empty( trim( $column['email'] ) ) ) : ?>
						<div class="item">
							<input type="email" class="form-controll" name="email" placeholder="<?php echo $column['email']; ?>" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>>
						</div>
					<?php endif; ?>


					<?php if ( 'seggiolino' === $column['acf_fc_layout'] && ! empty( $column['seggiolino'] ) ) : ?>
						<div class="item seggiolino">
							<span><?php echo __( 'Hai bisogno di un seggiolino?', 'ncc-treviso-child' ); ?></span>
							<div class="scelte">
								<label>
									<input type="radio" name="seggiolino" value="no" <?php echo ( 'no' === $column['seggiolino'] ? 'checked' : '' ); ?>>
									<span><?php echo __( 'No', 'ncc-treviso-child' ); ?></span>
								</label>
								<label>
									<input type="radio" name="seggiolino" value="yes" <?php echo ( 'yes' === $column['seggiolino'] ? 'checked' : '' ); ?>>
									<span><?php echo __( 'Si', 'ncc-treviso-child' ); ?></span>
								</label>
							</div>
						</div>
					<?php endif; ?>


					<?php if ( 'description' === $column['acf_fc_layout'] && ! empty( trim( $column['description'] ) ) ) : ?>
						<div class="item">
							<textarea class="form-controll" name="<?php echo apply_filters( 'text_to_attr', $column['description'] ); ?>" placeholder="<?php echo $column['description']; ?>" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>></textarea>
						</div>
					<?php endif; ?>


					<?php if ( 'bagagli' === $column['acf_fc_layout'] && ( ! empty( $column['descrizione_bagagli'] ) || ! empty( $column['foto_bagagli'] ) ) ) : ?>

						<div class="item bagagli <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>">

							<?php if ( ! empty( $column['descrizione_bagagli'] ) ) : ?>
								<div class="descrizione_bagagli">
									<textarea class="form-controll" name="descrizione_bagagli" placeholder="Descrizione bagagli o pure carica una foto"></textarea>
								</div>
							<?php endif; ?>

							<?php if ( ! empty( $column['foto_bagagli'] ) ) : ?>
								<div class="foto_bagagli">

									<div class="content">
										<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path></svg>
										<label>
											<input type="file" name="foto_bagagli[]" accept=".jpg,.jpeg,.png,.bmp,.tiff,.raw" multiple="true">
											<div class="select-btn">Select</div>
										</label>
										<div class="title">or drop files here</div>
									</div>

									<div class="thumbnails"></div>

								</div>
							<?php endif; ?>
						</div>
					<?php endif; ?>


					<?php if ( 'privacy_policy' === $column['acf_fc_layout'] && ! empty( trim( $column['privacy_policy'] ) ) ) : ?>
						<div class="item privacy_policy">
							<?php echo $column['privacy_policy']; ?>
							<label>
								<input type="checkbox" name="privacy_policy" <?php echo ( ! empty( $column['required'] ) ? 'required' : '' ); ?>>
								<span><?php echo __( 'Acconsento', 'ncc-treviso-child' ); ?></span>
							</label>
						</div>
					<?php endif; ?>

				<?php endforeach; ?>

				<?php if ( count( $form ) === $key + 1 ) : ?>
					<div class="item">
						<button type="button" class="submit btn-primary"><?php echo __( 'Invia', 'ncc-treviso-child' ); ?></button>
					</div>
				<?php else : ?>
					<div class="item">
						<button type="button" class="next-step btn-primary"><?php echo __( 'Avanti', 'ncc-treviso-child' ); ?></button>
					</div>
				<?php endif; ?>
			</div>

		<?php endforeach; ?>
	</form>
</div>
<?php endif; ?>
```

## Front-end script (to work on backend you need use mutation observer)
   *simple vanilla js script*
```js
const prenota_ora = document.querySelectorAll('.container-prenota-ora')

if (prenota_ora.length) {

    const getNodeIndex = elm => [...elm.parentNode.children].indexOf(elm)


    /**
     * The function `hundle_privacy` checks if a policy checkbox is checked and adds/removes an error
     * class accordingly.
     * @param policy - The `policy` parameter is a reference to an HTML input element, specifically a
     * checkbox.
     * @returns a boolean value. If the policy is checked and the policy container has the class
     * 'error', it will return true. If the policy is not checked, it will add the class 'error' to the
     * policy container and return false. If the policy is not provided or does not meet the
     * conditions, it will return false.
     */
    const hundle_privacy = (policy) => {

        if (policy) {

            const policy_container = policy.closest('.item')

            if (policy.checked) {

                policy_container.classList.remove('error')
                return true

            } else {

                policy_container.classList.add('error')
                return false
            }
        }

        return false
    }

    const hundle_description = (description, update_dom = true) => {

        const description_container = description.closest('.item')

        if ('' === description.value.trim()) {

            update_dom && description_container.classList.add('error')
            return false

        } else {

            update_dom && description_container.classList.remove('error')
            return true
        }
    }

    /**
     * The function `active_step` updates the active step and tab in a multi-step form.
     * @param container - The container is the element that contains the multi-steps and prenota-ora
     * elements. It is the parent element that holds all the steps and tabs.
     * @param multi_steps - multi_steps is an array of DOM elements representing the steps in a
     * multi-step process.
     * @param step - The `step` parameter is the current step element that needs to be activated.
     */
    const active_step = (container, multi_steps, step) => {

        const old_step = container.querySelector('.multi-steps li.is-active')
        const index = getNodeIndex(step)

        if (getNodeIndex(old_step) > index) {
            old_step.classList.remove('is-active')
            multi_steps[index].classList.add('is-active')

            const old_tab = container.querySelector('.prenota-ora .row.is-active')
            old_tab.classList.remove('is-active')

            const tabs = container.querySelectorAll('.prenota-ora .row')
            tabs[index].classList.add('is-active')
        }
    }


    const validate_tab = (tab) => {

        let validity = true
        const required = tab.querySelectorAll('[required]')
        const bagagli = tab.querySelector('.bagagli.required')

        for (let i = 0; i < required.length; i++) {

            if ('' === required[i].value.trim()) {
                validity = false
                required[i].classList.add('error')
            } else {
                required[i].classList.remove('error')
            }
        }

        if (bagagli) {

            const description = bagagli.querySelector('[name="descrizione_bagagli"]')
            const photos = bagagli.querySelector('.thumbnails')

            if (!hundle_description(description, !photos.children.length) && !photos.children.length) {
                validity = false
            }
        }

        return validity
    }

    /**
     * The function `handle_next_tab` is used to handle the navigation between tabs in a multi-step form.
     * @param container - The container parameter is the element that contains the multi-step tabs. It is
     * used to find the current step and tab, as well as the next step and tab.
     * @param next - The `next` parameter is the element that triggers the function when clicked. It is the
     * button or link that the user clicks on to move to the next tab.
     */
    const handle_next_tab = (container, next) => {

        const curent_tab = next.parentNode.parentNode
        const next_tab = curent_tab.nextElementSibling

        if (validate_tab(curent_tab)) {

            curent_tab.classList.remove('is-active')
            next_tab.classList.add('is-active')

            const curent_step = container.querySelector('.multi-steps li.is-active')
            const next_step = curent_step.nextElementSibling

            curent_step.classList.remove('is-active')
            next_step.classList.add('is-active')
        }
    }


    /**
     * The function `get_labels_values_by_name` retrieves the labels and values of form elements with a
     * specific name and returns them in an array.
     * @param name - The `name` parameter is a string that represents the name attribute of the form
     * elements you want to retrieve labels and values for.
     * @param [callback] - The `callback` parameter is an array that is used to store the labels and
     * values of form items. It is an optional parameter and if not provided, it defaults to an empty
     * array `[]`.
     * @returns The function `get_labels_values_by_name` returns the `callback` array.
     */
    const get_labels_values_by_name = (container, name, callback = []) => {

        const form_item = container.querySelectorAll(`[name="${name}"]`)

        form_item.forEach(async item => {

            if (
                'file' !== item.getAttribute('type') &&
                'radio' !== item.getAttribute('type') &&
                'checkbox' !== item.getAttribute('type') &&
                'hidden' !== item.getAttribute('type')
            ) {
                callback.push({
                    label: item.getAttribute('placeholder'),
                    value: item.value
                })
            }

            if (
                (
                    'radio' === item.getAttribute('type') ||
                    'checkbox' === item.getAttribute('type')
                ) && item.checked
            ) {

                const text = item.closest('.item').textContent

                callback.push({
                    label: text.replace(/\t?\n|\t/g, ''),
                    value: item.value
                })
            }
        })

        return callback
    }


    /**
     * The function `buld_request` sends a form data to a specified URL using an HTTP request.
     * @param container - The `container` parameter is the element that contains the form with the
     * class name "prenota-ora". It is used to select the form element and retrieve its data.
     */
    const buld_request = (submit, policy, container) => {

        const curent_tab = submit.parentNode.parentNode
        const policy_validity = hundle_privacy(policy)

        submit.classList.add('loading')

        if (validate_tab(curent_tab) && policy_validity) {

            const form = container.querySelector('.prenota-ora')
            const formData = new FormData(form)

            let label_val = []
            Array.from(formData).forEach(([key, val]) => label_val = get_labels_values_by_name(container, key, label_val))

            formData.append('ncc_label_form_data', JSON.stringify(label_val));

            sendHttpForm({
                url: json_url + 'prenota-ora',
                data: formData,
            }).then(res => {

                res = JSON.parse(res)

                if ('success' === res.status) {
                    container.innerHTML = (
                        `<div class="message success">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zM369 209L241 337c-9.4 9.4-24.6 9.4-33.9 0l-64-64c-9.4-9.4-9.4-24.6 0-33.9s24.6-9.4 33.9 0l47 47L335 175c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9z"></path></svg>
                            <p>${res.message}</p>
                        </div>`
                    )
                } else {
                    container.innerHTML = (
                        `<div class="message error">
                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zM175 175c9.4-9.4 24.6-9.4 33.9 0l47 47 47-47c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9l-47 47 47 47c9.4 9.4 9.4 24.6 0 33.9s-24.6 9.4-33.9 0l-47-47-47 47c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l47-47-47-47c-9.4-9.4-9.4-24.6 0-33.9z"/></svg>
                            <p>${res.message}</p>
                        </div>`
                    )
                }

            }).catch(e => {
                console.log(e);
            });
        }
    }


    /**
     * The function `handle_phone_number` takes a phone number input and removes any non-numeric
     * characters except for '+', '(', ')', and ' '.
     * @param phone_number - The `phone_number` parameter is the input field or element that contains
     * the phone number.
     */
    const handle_phone_number = (phone_number) => {

        const validated = phone_number.value.split('').filter(char => !isNaN(Number.parseInt(char)) || '+' === char || '(' === char || ')' === char || ' ' === char)

        phone_number.value = validated.join('')
    }


    /**
     * The function `hundle_photos` takes in a list of files, creates a div element for each file with
     * an image and name, and appends it to a container element.
     * @param files - The `files` parameter is an array-like object that contains the files selected by
     * the user through an input element of type "file".
     */
    const hundle_photos = (container, bagagli_photos) => {

        const photos_container = bagagli_photos.closest('.item')
        const files = bagagli_photos.files;

        Array.from(files).forEach(async file => {

            const item = document.createElement('div')
            const name = document.createElement('span')
            const img = document.createElement('img')

            item.classList.add('item-bagagli')
            name.classList.add('name-image')

            const reader = new FileReader()
            reader.onload = () => {

                name.textContent = file.name
                img.src = reader.result;

                item.append(img)
                item.append(name)

                const thumbnails = container.querySelector('.thumbnails')
                thumbnails && thumbnails.append(item)

                photos_container.classList.contains('error') && photos_container.classList.remove('error')
            }

            reader.readAsDataURL(file);
        })
    }




    /**
     * The function "hundle_form" handles the navigation and interaction with a multi-step form.
     * @param container - The `container` parameter is the element that contains the form. It is used
     * to select and manipulate elements within the form.
     */
    const hundle_form = (container) => {

        const submit = container.querySelector('.submit')

        const policy = container.querySelector('[name="privacy_policy"]')
        policy && policy.addEventListener('change', ev => hundle_privacy(policy), false)

        const descriptions = container.querySelectorAll('[name="descrizione_bagagli"]')
        descriptions && descriptions.forEach(async description => description.addEventListener('keyup', ev => hundle_description(description), false))

        const multi_steps = container.querySelectorAll('.multi-steps li')
        multi_steps.forEach(async step => step.addEventListener('click', ev => active_step(container, multi_steps, step), false))

        const nexts = container.querySelectorAll('.next-step')
        nexts.forEach(async next => next.addEventListener('click', ev => handle_next_tab(container, next), false))

        const phone_numbers = container.querySelectorAll('.phone-number')
        if (phone_numbers.length) {
            phone_numbers.forEach(async phone_number => phone_number.addEventListener('keyup', ev => handle_phone_number(phone_number), false))
        }

        const bagagli_photos = container.querySelector('[name="foto_bagagli[]"]')
        if (bagagli_photos) {
            bagagli_photos.addEventListener('change', ev => hundle_photos(container, bagagli_photos), false)
        }

        submit.addEventListener('click', ev => buld_request(submit, policy, container), false)
    }

    prenota_ora.forEach(async container => hundle_form(container))
}
```


## exportation of block
   *import "acf-export-2023-11-26.json" json from ACF Pro plugin importer*
after import use the corect name of block and save
   https://monosnap.com/file/WteztuhKgWG4C7XBozVLrLnO4CxXrJ
