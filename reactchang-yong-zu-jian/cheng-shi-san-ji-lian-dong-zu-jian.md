```
import * as React from 'react';

import { Cascader } from 'antd';

import * as _ from 'lodash';

interface ProvinceCityCascadeProps {
    options: Array<any>;
    loadData: Function;
    placeholder?: string;
    requiredMessage?: string;
    disabled?: boolean;
    required?: boolean;
    defaultValue?: Array<any>;
    [random: string]: any;
}

interface ProvinceCityCascadeState {
    selectedCascadeValue: any;
    selectedCascadeArray: Array<any>;
    options: Array<any>;
    prov_id: any;
    city_id: any;
    area_id: any;
    [random: string]: any;
}

export default class ProvinceCityCascade extends React.Component<ProvinceCityCascadeProps, ProvinceCityCascadeState>{

    static defaultProps: ProvinceCityCascadeProps = {
        requiredMessage: '请选择区域',
        placeholder: '请选择区域',
        options: [],
        loadData: () => { },
        required: false,
        disabled: false,
    };

    constructor(props) {
        super(props);
        this.state = {
            selectedCascadeValue: this.props.defaultValue || [],
            selectedCascadeArray: [],
            options: this.props.options,
            prov_id: undefined,
            city_id: undefined,
            area_id: undefined,
        }
        this.cascadeOnChange = this.cascadeOnChange.bind(this);
    }

    componentWillReceiveProps(nextProps, nextContext) {
        const areaList = nextProps.options;
        const defaultValue = nextProps.defaultValue;
        if (!defaultValue) {
            return;
        }
        if (!_.isEqual(this.props.defaultValue.sort(), nextProps.defaultValue.sort())) {
            let [prov_id, city_id, area_id] = nextProps.defaultValue;
            this.setState({ options: areaList, selectedCascadeValue: nextProps.defaultValue,prov_id,city_id,area_id });
        }
        else {
            this.setState({ options: areaList });
        }
    }

    cascadeOnChange(value, selectedOptions) {
        if (value.length < 1) {
            this.setState({
                selectedCascadeValue: [],
                selectedCascadeArray: [],
                prov_id: undefined,
                city_id: undefined,
                area_id: undefined,
            });
            return;
        }
        value.forEach((item, index) => {
            switch (index) {
                case 0:
                    this.setState({
                        selectedCascadeValue: value,
                        prov_id: item,
                    });
                    break;
                case 1:
                    this.setState({
                        selectedCascadeValue: value,
                        city_id: item,
                    });
                    break;
                case 2:
                    this.setState({
                        selectedCascadeValue: value,
                        area_id: item,
                    });
                    break;
                default:
                    break;
            }
        })

    }

    render() {
        return (
            <span>
                <Cascader
                    style={this.props.style}
                    value={this.state.selectedCascadeValue}
                    placeholder={this.props.placeholder}
                    disabled={this.props.disabled}
                    options={this.state.options}
                    onChange={this.cascadeOnChange}
                    size={'large'}
                    changeOnSelect
                />
                &nbsp;&nbsp;
                {!this.props.disabled && this.props.required && this.state.selectedCascadeValue.length < 1 && <div style={{ color: 'red' }}>{this.props.requiredMessage}</div>}
            </span>
        );
    }
}
```