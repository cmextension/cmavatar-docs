.. _ref-display:

==============
Display avatar
==============

In profile
----------

To display avatar in Joomla!'s profile view, you will need to override the layout of Users component.

Copy the file components/com_users/views/profile/tmpl/default_custom.php to this location: templates/YOUR_CURRENT_TEMPLATE/html/com_users/profile/default_custom.php.

Below these 2 lines::

	<?php else : ?>
		<?php echo JHtml::_('users.value', $field->value); ?>

You add::

	<?php elseif ($field->type == 'CMAvatar') : ?>
		<?php echo $field->value; ?>

This is the whole file::

	<?php
	/**
	 * @package     Joomla.Site
	 * @subpackage  com_users
	 *
	 * @copyright   Copyright (C) 2005 - 2015 Open Source Matters, Inc. All rights reserved.
	 * @license     GNU General Public License version 2 or later; see LICENSE.txt
	 */

	defined('_JEXEC') or die;

	JLoader::register('JHtmlUsers', JPATH_COMPONENT . '/helpers/html/users.php');
	JHtml::register('users.spacer', array('JHtmlUsers', 'spacer'));


	$fieldsets = $this->form->getFieldsets();
	if (isset($fieldsets['core']))   unset($fieldsets['core']);
	if (isset($fieldsets['params'])) unset($fieldsets['params']);

	foreach ($fieldsets as $group => $fieldset): // Iterate through the form fieldsets
		$fields = $this->form->getFieldset($group);
		if (count($fields)):
	?>
	<?php //if ($this->params->get('show_tags')) : ?>
			<?php //$this->tagLayout = new JLayoutFile('joomla.content.tags'); ?>
			<?php //echo $this->tagLayout->render($this->tags); ?>
		<?php // endif; ?>

	<fieldset id="users-profile-custom" class="users-profile-custom-<?php echo $group; ?>">
		<?php if (isset($fieldset->label)): // If the fieldset has a label set, display it as the legend. ?>
		<legend><?php echo JText::_($fieldset->label); ?></legend>
		<?php endif; ?>
		<dl class="dl-horizontal">
		<?php foreach ($fields as $field) :
			if (!$field->hidden && $field->type != 'Spacer') : ?>
			<dt><?php echo $field->title; ?></dt>
			<dd>
				<?php if (JHtml::isRegistered('users.' . $field->id)) : ?>
					<?php echo JHtml::_('users.' . $field->id, $field->value); ?>
				<?php elseif (JHtml::isRegistered('users.' . $field->fieldname)) : ?>
					<?php echo JHtml::_('users.' . $field->fieldname, $field->value); ?>
				<?php elseif (JHtml::isRegistered('users.' . $field->type)) : ?>
					<?php echo JHtml::_('users.' . $field->type, $field->value); ?>
				<?php elseif ($field->type == 'CMAvatar') : ?>
					<?php echo $field->value; ?>
				<?php else : ?>
					<?php echo JHtml::_('users.value', $field->value); ?>
				<?php endif; ?>
			</dd>
			<?php endif; ?>
		<?php endforeach; ?>
		</dl>
	</fieldset>
		<?php endif; ?>
	<?php endforeach; ?>

Now you can see the avatar in your profile view.

.. image:: ../images/profile_view_fixed.jpg

In another extension
--------------------

To display avatar in another extension, eg. your community component, your blog component, etc... you need to get the avatar file name from the database and display it by using PHP.

We have a small helper for that. You can use it like this::

	require_once JPATH_PLUGINS . '/user/cmavatar/helper.php';

	$avatar = PlgUserCMAvatarHelper::getAvatar($userId);

$userId is the variable which holds the ID of the user of you want to get his/her avatar.

The result in $avatar is the path to the avatar of the user, for example "images/avatars/4d99b38d96ed65caf39fde008528f185.jpg", you can do whatever you want with it like put it into <img> HTML element. If user doesn't have avatar, the result is empty.