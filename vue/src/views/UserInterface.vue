<!--
 - @copyright Copyright (c) 2019 Joas Schilling <coding@schilljs.com>
 -
 - @author Joas Schilling <coding@schilljs.com>
 -
 - @license GNU AGPL version 3 or any later version
 -
 - This program is free software: you can redistribute it and/or modify
 - it under the terms of the GNU Affero General Public License as
 - published by the Free Software Foundation, either version 3 of the
 - License, or (at your option) any later version.
 -
 - This program is distributed in the hope that it will be useful,
 - but WITHOUT ANY WARRANTY; without even the implied warranty of
 - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 - GNU Affero General Public License for more details.
 -
 - You should have received a copy of the GNU Affero General Public License
 - along with this program. If not, see <http://www.gnu.org/licenses/>.
 -
 -->

<template>
	<div id="content">
		<div id="app-navigation">
			<app-content app-name="settings" :navigation-class="{ 'icon-loading': loading }">
				<template #navigation>
					<!--<app-navigation-new button-id="new-user-button" :text="t('settings','New user')" button-class="icon-add" />-->
					<ul id="spreedme-room-list" class="with-icon">
						<app-navigation-item v-for="item in menu" :key="item.key" :item="item" />
					</ul>
				</template>
			</app-content>
		</div>
		<router-view />
	</div>
</template>

<script>

import {
	AppContent,
	// AppNavigationNew,
	AppNavigationItem
} from 'nextcloud-vue'

export default {
	name: 'UserInterface',

	components: {
		AppContent,
		// AppNavigationNew,
		AppNavigationItem
	},

	data() {
		return {
			loading: {
				type: Boolean,
				default: true
			}
		}
	},

	computed: {
		activeRoomToken() {
			return this.$route.params.token
		},
		filters() {
			return this.$store.getters.getConversations
		},
		menu() {
			console.log(this.conversations)

			const items = [
				{
					id: 'token',
					router: {
						name: 'conversation-token',
						params: {
							token: 'token'
						}
					},
					classes: ['room-list-item'],
					iconUrl: OC.imagePath('spreed', 'app-dark.svg'),
					text: 'filter.name'
				}
			]

			console.log(items)

			return items
		}
	},

	watch: {
		// watch url change and group select
		token(/* val, old */) {
			this.$store.commit('reset')
			// this.$store.dispatch('fetchActivities', this.filter);
			// this.$refs.infiniteLoading.$emit('$InfiniteLoading:reset');
		}
	},

	beforeMount() {
		this.$store.dispatch('fetchConversations')
		// this.servers = OCP.InitialState.loadState('talk', 'turn_servers')
	},

	methods: {
		goBack() {
			window.history.length > 1
				? this.$router.go(-1)
				: this.$router.push('/')
		}
	}
}
</script>
